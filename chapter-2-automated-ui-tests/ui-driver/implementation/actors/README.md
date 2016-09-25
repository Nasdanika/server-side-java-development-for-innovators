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

Below is the initial implementation of the ``Customer`` actor. As with the ``Guest`` implementation it currently only has functionality to demonstrate basic UI flows.
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

UI Driver abstracts tests from low-level details of user interfaces. 
Essentially, the UI Driver is an [embedded/internal DSL](https://en.wikipedia.org/wiki/Domain-specific_language#Domain-specific_language_topics) in which tests are written.

In Nasdanika WebTest the UI driver consists of 4 components in two logical layers:

* Actors - express coarse "business" functionality, e.g. "Log-In" or "Submit Payment". 
  * Specification - factory and actor interfaces. May or may not depend on the page specification. 
  * Implementation - implementations of the specification written in terms of page specifications. Page implementations are wired at runtime via [OSGi Declarative Services](http://www.vogella.com/tutorials/OSGiServices/article.html#the-osgi-declarative-services-functionality).
* Pages - represent low-level UI functionality, e.g. "Enter User ID", "Enter Password", "Click Sign-On Button" for the "Log-In" actor method.
  * Specification - factory and page interfaces.
  * Implementation - implementation of the specification, typically using [Selenium WebDriver](http://www.seleniumhq.org/projects/webdriver/) to automate web browsers and/or mobile devices.  


![webtest](https://github.com/Nasdanika/server/wiki/webtest.png)


We already have a story model with users, stories, scenarios, steps. 

![story-model.png](story-model.png)

With this information creation of the UI Driver specification and tests implementations is rather straightforward.

We will create:

* Guest and Customer actors for Guest and Customer users.
* Pages for states.   
* Actor methods for stories.
* Actor or page methods for steps.

As with the tests we will use [@Link](http://www.nasdanika.org/server/apidocs/org.nasdanika.webtest/target/site/apidocs/org/nasdanika/webtest/Link.html) annotation to establish traceability between the UI driver interfaces and the story model elements.


### Specifications

First we shall create actor and page specifications.

#### Actors

WebTest actor interfaces correspond to protagonists (Roles, Actors, Users, Systems) in the story model with interface methods corresponding to stories. 

As stories transition between states and states are represented by pages, it might seem logical for actor methods to return page interfaces. However, it introduces a dependency between the actor and page specifications, which might be not be desirable. The current page can be accessed via ``Actor.getCurrentPage()`` method.

##### Factory

Factory is responsible for creation of instances of ``Guest`` actor. 

```java
package org.nasdanika.bank.ui.driver.actors;

import org.nasdanika.bank.ui.driver.pages.NasdanikaBankPageFactory;
import org.openqa.selenium.WebDriver;

public interface NasdanikaBankActorFactory {
	
	NasdanikaBankPageFactory getPageFactory();
	
	Guest createGuest(WebDriver webDriver);

}
```   

##### Guest

Below is the ``Guest`` interface. It has a link to the ``Guest`` user in the story model.
Interface methods correspond to the ``Guest`` user stories and have links to the corresponding stories.
There is also a helper method to create a customer for testing.

```java
package org.nasdanika.bank.ui.driver.actors;

import org.nasdanika.webtest.Actor;
import org.nasdanika.webtest.Description;
import org.nasdanika.webtest.Link;
import org.openqa.selenium.WebDriver;

@Description("Guest, a.k.a. Vistior or 'Unauthenticated principal'")
@Link(
		type = "User@urn:org.nasdanika.story", 
		value = "org.nasdanika.bank.app/Bank.nasdanika_story#guest")
public interface Guest extends Actor<WebDriver> {
		
	/**
	 * Navigates to the home page. The page can be accessed by invoking <code>getCurrentPage</code> method.
	 */
	@Link(
			type = "Scenario@urn:org.nasdanika.story", 
			value = "org.nasdanika.bank.app/Bank.nasdanika_story#guest.navigate-home")
	@Description("Navigate to the landing page")
	void navigateToHomePage();
	
	/**
	 * Logs in to the system
	 * @param userId User ID
	 * @param password Password
	 * @return Instance of the customer upon successful log-in.
	 * @throws IllegalArgumentException If login fails. <code>getCurrentPage()</code> method can be used in this case to 
	 * access to the {@link GuestHomePage} for further analysis, e.g. to read the error message.
	 */
	@Link(
			type = "Scenario@urn:org.nasdanika.story", 
			value = "org.nasdanika.bank.app/Bank.nasdanika_story#guest.log-in")
	@Description("Log in to the system")
	Customer logIn(String userId, String password);

	/**
	 * Registers a new customer
	 * @return Instance of the customer upon successful registration.
	 * @throws IllegalArgumentException If registration fails. <code>getCurrentPage()</code> method can be used in this case to 
	 * access to the current page for further analysis.
	 */
	@Link(
			type = "Scenario@urn:org.nasdanika.story", 
			value = "org.nasdanika.bank.app/Bank.nasdanika_story#guest.sign-up")
	@Description("Registers customer")
	Customer register(/* TODO - registration data */);
	
	/**
	 * Helper method to simplify testing.
	 * @param alias Customer alias. If null, then the alias is set to "Customer".
	 * @return
	 * @throws IllegalArgumentException
	 */
	@Description("Generates random but unique customer registration data and registers a customer")
	TestCustomer createTestCustomer(String alias);

}
```

##### Customer


``Customer`` interface corresponds to the ``Customer`` user in the story model and is linked to it. 
It contains nested interfaces representing accounts, statements and transactions.  

```java
package org.nasdanika.bank.ui.driver.actors;

import java.util.List;

import org.nasdanika.webtest.Actor;
import org.nasdanika.webtest.Delegate;
import org.nasdanika.webtest.Description;
import org.nasdanika.webtest.Link;
import org.openqa.selenium.WebDriver;

@Description("Logged-in customer, authenticated principal")
@Link(
		type = "User@urn:org.nasdanika.story", 
		value = "org.nasdanika.bank.app/Bank.nasdanika_story#customer")

public interface Customer extends Actor<WebDriver> {
	
	/**
	 * A delegate actor for working with the account data.
	 * @author Pavel Vlasov
	 *
	 */
	@Link(
			type = "Scenario@urn:org.nasdanika.story", 
			value = "org.nasdanika.bank.app/Bank.nasdanika_story#customer.account-details.load")
	@Description("Load account details")
	interface Account extends Actor<WebDriver>, Delegate {
						
		List<Statement> getStatements();
		
		/* TODO - account attributes - balance, next statement closing date, ...
		 *  Maybe a hierarchy of accounts 
		 *  - base,
		 *  - checking - just extends the base, or deposit account interface with checking and savings as sub-interfaces - move to own package and classes in this case.  
		 *  - credit 
		 *    - credit line - total/available, 
		 *    - cash credit line - total/available, 
		 *    - last payment, 
		 *    - last payment date
		 */
		
	}
	
	/**
	 * A delegate actor for working with the statement data.
	 * @author Pavel Vlasov
	 *
	 */
	@Link(
			type = "Scenario@urn:org.nasdanika.story", 
			value = "org.nasdanika.bank.app/Bank.nasdanika_story#customer.account-details.select-statement")
	@Description("Select account statement")
	interface Statement extends Actor<WebDriver>, Delegate {
		
		List<Transaction> getTransactions();
		
	} 
	
	/**
	 * A delegate actor for working with the transaction data.
	 * @author Pavel Vlasov
	 *
	 */
	interface Transaction extends Actor<WebDriver>, Delegate {
		
		
		
	} 
	
	/**
	 * Retrieves a list of customer accounts.
	 */
	@Link(
			type = "Scenario@urn:org.nasdanika.story", 
			value = "org.nasdanika.bank.app/Bank.nasdanika_story#customer.account-summary")
	@Description("Retrieve a list of customer accounts")
	List<Account> getAccounts();
	
	/**
	 * Logs out.
	 * @return A guest instance upon successful log-out.
	 * @throws IllegalStateException If log-out fails. 
	 */
	@Link(
			type = "Scenario@urn:org.nasdanika.story", 
			value = "org.nasdanika.bank.app/Bank.nasdanika_story#customer.sign-out")
	@Description("Log out from the system")
	Guest logOut();


}
```

###### TestCustomer

``TestCustomer`` is a ``Customer`` specialization for testing purposes. 

```java
package org.nasdanika.bank.ui.driver.actors;

import org.nasdanika.webtest.Aliased;

/**
 * Helper class to simplify testing. Tests customers are created with
 * random but unique registration data. 
 * @author Pavel Vlasov
 *
 */
public interface TestCustomer extends Customer, Aliased {
	
	/*
	 * Maybe a better way is to have an interface RegistrationInfo and a method getRegistrationInfo(). 
	 */
	
	/**
	 * @return User ID
	 */
	String getUserId();

	/**
	 * @return Password
	 */
	String getPassword();
	
	/**
	 * Removes test customer record.
	 */
	void dispose();

}
```


#### Pages

##### Guest

Pages correspond to states in the story model. For ``Guest`` user we have a template "abstract" state and two concrete states. 
We will put guest pages into ``package org.nasdanika.bank.ui.driver.pages.guest`` package.

###### Template

The template interface extends ``NasdanikaBankPage`` and currently doesn't have any methods. 

```java
package org.nasdanika.bank.ui.driver.pages.guest;

import org.nasdanika.bank.ui.driver.pages.NasdanikaBankPage;

public interface Template extends NasdanikaBankPage {

}
```

###### Home page

The home page interface extends the template interface and provides methods to enter login, password, click the "Log In" and access an error
message text if log in fails.

```java
package org.nasdanika.bank.ui.driver.pages.guest;

import org.nasdanika.bank.ui.driver.pages.NasdanikaBankPage;

public interface Home extends Template {
	
	void setLogin(String login);
	
	void setPassword(String password);
	
	NasdanikaBankPage clickLogInButton();
	
	String getLogInErrorMessage();

}
```

``clickLogInButton()`` method returns a page, which can be the customer home page if log-in was successful, or the guest home page if 
log-in failed. 

###### Registration form

Currently registration is out of scope, so the registration page doesn't declare any methods.

```java
package org.nasdanika.bank.ui.driver.pages.guest;

public interface RegistrationForm extends Template {

}
```

##### Customer

Customer has a template "abstract" state, accounts summary (home) state, and account details state. 
We will create page interfaces corresponding to these states in ``org.nasdanika.bank.ui.driver.pages.customer`` package.

###### Template

The template has ``clickLogOut()`` method, which returns the Guest home page upon successful log-out. 
It also has ``goHome()`` method for navigating to the accounts summary page using the top navigation menu. 

```java
package org.nasdanika.bank.ui.driver.pages.customer;

import org.nasdanika.bank.ui.driver.pages.NasdanikaBankPage;

public interface Template extends NasdanikaBankPage {
	
	org.nasdanika.bank.ui.driver.pages.guest.Home logOut();
	
	Home goHome();

}
```

###### Home

The customer home page has a list of customer accounts with each item in the list displaying account label, 
which is the accounts's product name with the last 4 digits of the account number, and the current balance:

![customer-home-web](../../../chapter-1-user-stories/customer-home-web.png)

Click on the account label navigates to the account details page.

The home page interface declares a nested interface ``AccountSummary`` and a method returning a list of account summaries.

```java
package org.nasdanika.bank.ui.driver.pages.customer;

import java.math.BigDecimal;
import java.util.List;

public interface Home extends Template {
	
	interface AccountSummary {
		
		String getLabel();
		
		BigDecimal getBalance();
		
		AccountDetails navigateToDetails();
		
	}	
	
	List<AccountSummary> getAccountSummaries();
	
}
```

###### Account Details

Account details page provides additional details about the account, such as next statement closing date, 
and displays transactions for a particular account statement. 
There is a drop-down which allows to select a statement to display:  
 
![customer-home-web](../../../chapter-1-user-stories/account-details-web.png)

We are not that interested in the account summary header right now because our goal is to have a UI driver which can
demonstrate navigation between different parts of the application. 
Therefore, the account details page interface focuses on selection of statements for now:

```java
package org.nasdanika.bank.ui.driver.pages.customer;

import java.math.BigDecimal;
import java.util.Date;
import java.util.List;

public interface AccountDetails extends Template {
	
	interface Statement {
		
		interface Transaction {
			
			Date getDate();
			String getDescription();
			BigDecimal getAmount();
			BigDecimal getRunningBalance();
			
		}
		
		String getLabel();
		
		List<Transaction> select();
		
	}
	
	List<Statement> getStatements();

}
```  

### Tests implementations

factory injection...

...  



JavaDoc links to actor interfaces from story users. Bi-directional link to test results actors.

page -> state
actor -> user
test method -> scenario
actor/page method -> step


Mention Mockito or use it???