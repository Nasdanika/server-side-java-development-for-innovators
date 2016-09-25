# Tests implementations

We will use top-down approach in implementation. The first step is to implement tests using the actor specification DSL.

## Guest

### Log-in

The body of the log-in test method is shown below:

```java
Guest guest = actorFactory.createGuest(getWebDriver());
try (TestCustomer testCustomer = guest.createTestCustomer("John Doe")) {
	guest.logIn(testCustomer.getUserId(), testCustomer.getPassword());
}
``` 

The test creates a test customer in a try with resource block and then uses the test customer credentials to log-in.
At this point the purpose of our tests is not to test, but to drive the UI flow. 
Therefore there are no assertions in the tests, they will come into the picture at later stages.

### Navigate to home page

This test is just a one-liner:

```java
actorFactory.createGuest(getWebDriver()).navigateToHomePage();
```

## Customer

### CustomerTestBase

Customer tests require a logged-in customer. Logging in in each test method will lead to code duplication and also will 
create noise in test reports. To avoid this we will create a base class for customer tests:

```java
package org.nasdanika.bank.tests.customer;

import org.junit.After;
import org.junit.Before;
import org.nasdanika.bank.tests.NasdanikaBankTestBase;
import org.nasdanika.bank.ui.driver.actors.Customer;
import org.nasdanika.bank.ui.driver.actors.Guest;
import org.nasdanika.bank.ui.driver.actors.TestCustomer;
import org.nasdanika.webtest.Category;

@Category("Customer")
public class CustomerTestBase extends NasdanikaBankTestBase {
	
	private TestCustomer testCustomer;
	
	private Customer customer;
	
	protected Customer getCustomer() {
		return customer;
	}
	
	@Before
	@Override
	public void setUp() throws Exception {
		super.setUp();
		Guest guest = actorFactory.createGuest(getWebDriver());
		testCustomer = guest.createTestCustomer("John Doe");
		customer = guest.logIn(testCustomer.getUserId(), testCustomer.getPassword());
	}
	
	@After
	public void tearDown() throws Exception {
		testCustomer.close();
		quitDriver();
	}
			
}
```

This class creates a test customer in its set-up method and closes it in the tear-down method.

### AccountDetailsTests

For the sake of brevity we will take a look at only one customer test class - ``AccountDetailsTests``:

```java
package org.nasdanika.bank.tests.customer;

import java.util.List;
import java.util.Random;

import org.junit.Test;
import org.nasdanika.bank.ui.driver.actors.Customer.Account;
import org.nasdanika.bank.ui.driver.actors.Customer.Statement;
import org.nasdanika.webtest.Link;
import org.nasdanika.webtest.Sketch;

@Link(
		type = "Story@urn:org.nasdanika.story", 
		value = "org.nasdanika.bank.app/Bank.nasdanika_story#customer.account-details")
public class AccountDetailsTests extends CustomerTestBase {
	
	@Test
	@Sketch(
			before="bundle://org.nasdanika.bank.app/sketches/customer-home-web.png",
			after="bundle://org.nasdanika.bank.app/sketches/account-details-web.png",
			selector="sketch/web")
	@Sketch(
			before="bundle://org.nasdanika.bank.app/sketches/customer-home-mobile.png",
			after="bundle://org.nasdanika.bank.app/sketches/account-details-mobile.png",
			selector="sketch/mobile")
	@Link(
			type = "Scenario@urn:org.nasdanika.story", 
			value = "org.nasdanika.bank.app/Bank.nasdanika_story#customer.account-details.load")
	public void loadAccountDetails() throws Exception {
		List<Account> accounts = getCustomer().getAccounts();
		Random random = new Random();
		Account account = accounts.get(random.nextInt(accounts.size()));
		List<Statement> statements = account.getStatements();
	}

	@Test
	@Sketch(
			after="bundle://org.nasdanika.bank.app/sketches/account-details-web.png",
			selector="sketch/web")
	@Sketch(
			after="bundle://org.nasdanika.bank.app/sketches/account-details-mobile.png",
			selector="sketch/mobile")
	@Link(
			type = "Scenario@urn:org.nasdanika.story", 
			value = "org.nasdanika.bank.app/Bank.nasdanika_story#customer.account-details.select-statement")
	public void selectStatement() throws Exception {		
//		// Demo of unsupported parameter value.
//		if (getWebDriver() instanceof SketchWebDriver && "sketch/web".equals(((SketchWebDriver) getWebDriver()).getSelector())) {
//			WebTestUtil.ignore();
//		}
		
		List<Account> accounts = getCustomer().getAccounts();
		Random random = new Random();
		Account account = accounts.get(random.nextInt(accounts.size()));
		List<Statement> statements = account.getStatements();
		Statement statement = statements.get(statements.size()-1);
		statement.getTransactions();
	}	
		
}
```

Now the class extends ``CustomerTestBase`` and the ``@Category`` annotation was removed - it is inherited from the base class.

Test methods retrieve accounts list and then use a random account from the list to retrieve a list of statements.
The ``selectStatement()`` method retrieves a list of transactions from the last statement in the list.

## Summary

With test methods implemented in terms of the actor specifications we shall proceed to implementing the actors in terms of the page specifications.
