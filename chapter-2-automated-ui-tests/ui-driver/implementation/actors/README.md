# Actor implementations

In this section we will implement actor interfaces in terms of page specifications.

## Guest

This is the implementation of ``Guest`` actor interface:

```java
package org.nasdanika.bank.ui.driver.actors.impl;

import org.nasdanika.bank.ui.driver.actors.Customer;
import org.nasdanika.bank.ui.driver.actors.Guest;
import org.nasdanika.bank.ui.driver.actors.NasdanikaBankActorFactory;
import org.nasdanika.bank.ui.driver.actors.TestCustomer;
import org.nasdanika.bank.ui.driver.pages.customer.Home;
import org.nasdanika.webtest.Page;
import org.openqa.selenium.WebDriver;

class GuestImpl implements Guest {
	
	private NasdanikaBankActorFactory factory;
	private WebDriver webDriver;

	GuestImpl(NasdanikaBankActorFactory factory, WebDriver webDriver) {
		this.webDriver = webDriver;
		this.factory = factory;
	}
	
	private Page<WebDriver> currentPage;

	@Override
	public Page<WebDriver> getCurrentPage() {
		return currentPage;
	}

	@Override
	public void navigateToHomePage() {
		currentPage = factory.getPageFactory().navigateToGuestHomePage(webDriver);
	}

	@Override
	public Customer logIn(String userId, String password) {
		org.nasdanika.bank.ui.driver.pages.guest.Home guestHome = (org.nasdanika.bank.ui.driver.pages.guest.Home) currentPage;
		guestHome.setLogin(userId);
		guestHome.setPassword(password);
		// TODO - analyze resulting page - customer home or guest home with an error message if log-in failed.
		// For now assuming happy path.
		org.nasdanika.bank.ui.driver.pages.customer.Home customerHome = (Home) guestHome.clickLogInButton();
		return new CustomerImpl(factory, webDriver, customerHome);
	}

	@Override
	public Customer register() {
		throw new UnsupportedOperationException();
	}

	@Override
	public TestCustomer createTestCustomer(String alias) {
		// TODO - either use UI or communicate with the application by other means
		// to create a customer object. For now we create an anonymous instance with static data.
		return new TestCustomer() {

			@Override
			public void close() throws Exception {
				// NOP, in a real implementation - delete customer object from the application. 				
			}

			@Override
			public String getUserId() {
				return "john-doe";
			}

			@Override
			public String getPassword() {
				return "irrelevant";
			}
			
		};
	}

}
```

The ``logIn()`` method uses the Page specification API to set user ID, password and click the log-in button. Then 
it creates and returns an instanceof ``Customer`` actor.

``register()`` is not currently supported.

``createTestCustomer()`` instantiates and returns an anonymous instance of ``TestCustomer``. Later this method
should be elaborated to create a customer object in the application object store and ``TestCustomer.close()`` method shall delete the customer object.

## Customer

Below is the initial implementation of the ``Customer`` actor. As with the ``Guest`` implementation, currently it only has functionality to demonstrate basic UI flows.
More functionality will have to be added later to support more advanced flows and testing. 

```java
package org.nasdanika.bank.ui.driver.actors.impl;

import java.util.List;
import java.util.stream.Collectors;

import org.nasdanika.bank.ui.driver.actors.Customer;
import org.nasdanika.bank.ui.driver.actors.Guest;
import org.nasdanika.bank.ui.driver.actors.NasdanikaBankActorFactory;
import org.nasdanika.bank.ui.driver.pages.customer.AccountDetails;
import org.nasdanika.bank.ui.driver.pages.customer.Home;
import org.nasdanika.bank.ui.driver.pages.customer.Home.AccountSummary;
import org.nasdanika.bank.ui.driver.pages.customer.Template;
import org.nasdanika.webtest.Page;
import org.openqa.selenium.WebDriver;

class CustomerImpl implements Customer {

	private NasdanikaBankActorFactory factory;
	private Page<WebDriver> currentPage;
	private WebDriver webDriver;

	CustomerImpl(NasdanikaBankActorFactory factory, WebDriver webDriver, org.nasdanika.bank.ui.driver.pages.customer.Home homePage) {
		this.factory = factory;
		this.webDriver = webDriver;
		currentPage = homePage;
	}

	@Override
	public Page<WebDriver> getCurrentPage() {
		return currentPage;
	}
			
	private class AccountImpl implements Account {
		
		private AccountSummary summary;
		private AccountDetails details;

		AccountImpl(AccountSummary summary) {
			this.summary = summary;
		}

		@Override
		public Page<WebDriver> getCurrentPage() {
			return currentPage;
		}

		@Override
		public List<Statement> getStatements() {
			// TODO - analyze if the current details page is the page for this particular account
			// If yes - use data from it, if no - navigate to the customer home and then 
			// Navigate to the details.
			// For now we don't check account number on the details page.
			if (currentPage instanceof Home) {
				details = summary.navigateToDetails();
			}
			
			return details.getStatements().stream().map(s -> new StatementImpl(s)).collect(Collectors.toList());
		}
		
	}
	
	private class StatementImpl implements Statement {
		
		private org.nasdanika.bank.ui.driver.pages.customer.AccountDetails.Statement statement;

		StatementImpl(org.nasdanika.bank.ui.driver.pages.customer.AccountDetails.Statement statement) {
			this.statement = statement;
		}

		@Override
		public Page<WebDriver> getCurrentPage() {
			return currentPage;
		}

		@Override
		public List<Transaction> getTransactions() {
			// TODO
			throw new UnsupportedOperationException();
		}
		
	}

	@Override
	public List<Account> getAccounts() {
		return ((Home) currentPage).getAccountSummaries().stream().map(s -> new AccountImpl(s)).collect(Collectors.toList());
	}

	@Override
	public Guest logOut() {
		((Template) currentPage).logOut();
		return new GuestImpl(factory, webDriver);
	}

}

```

## Summary

Now we have our tests and actors implemented. The next step is to implement pages. 
