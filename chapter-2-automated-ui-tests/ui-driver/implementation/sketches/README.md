# Page implementations - Sketches

We are going to have two flavors of page implementations:

* The first one will have no functionality. The framework and will use sketch annotations on page interfaces as "screenshots"
* The other will actually drive the UI.
 
The factory will select which implementation to use based on the driver type.

We will start with the first flavor. It will use sketches we already have and we will modify a couple of them slightly.

## Factory

The factory creates an instance of sketch guest home:

```java
package org.nasdanika.bank.ui.driver.pages.impl;

import org.nasdanika.bank.ui.driver.pages.NasdanikaBankPageFactory;
import org.nasdanika.bank.ui.driver.pages.guest.Home;
import org.nasdanika.webtest.SketchWebDriver;
import org.openqa.selenium.WebDriver;
import org.osgi.service.component.ComponentContext;

public class NasdanikaBankPageFactoryImpl implements NasdanikaBankPageFactory {

	private String baseURL;
	
	public void activate(ComponentContext context) {
		baseURL = (String) context.getProperties().get("base-url");
		System.out.println("Page factory component activated with base URL '"+baseURL+"'");
	}
	
	public void setBaseURL(String baseURL) {
		this.baseURL = baseURL;
	}
	
	public String getBaseURL() {
		return baseURL;
	}

	@Override
	public Home navigateToGuestHomePage(WebDriver webDriver) {
		if (!(webDriver instanceof SketchWebDriver)) {
			webDriver.get(getBaseURL());			
		} 

		return createGuestHomePage(webDriver);
	}

	@Override
	public Home createGuestHomePage(WebDriver webDriver) {
		if (webDriver instanceof SketchWebDriver) {
			return new org.nasdanika.bank.ui.driver.pages.impl.sketch.guest.HomeImpl(webDriver);
		}
		
		throw new UnsupportedOperationException();
	}

}
```

## Guest pages

### Home

```java
package org.nasdanika.bank.ui.driver.pages.impl.sketch.guest;

import org.nasdanika.bank.ui.driver.pages.NasdanikaBankPage;
import org.nasdanika.bank.ui.driver.pages.impl.NasdanikaBankPageImpl;
import org.openqa.selenium.WebDriver;

public class HomeImpl extends NasdanikaBankPageImpl implements org.nasdanika.bank.ui.driver.pages.guest.Home {

	public HomeImpl(WebDriver webDriver) {
		super(webDriver);
	}

	@Override
	public void setLogin(String login) {
		// NOP		
	}

	@Override
	public void setPassword(String password) {
		// NOP		
	}

	@Override
	public NasdanikaBankPage clickLogInButton() {
		return new org.nasdanika.bank.ui.driver.pages.impl.sketch.customer.HomeImpl(getWebDriver());
	}

	@Override
	public String getLogInErrorMessage() {
		return null;
	}
	
}
```


## Customer pages

### Home 

```java
package org.nasdanika.bank.ui.driver.pages.impl.sketch.customer;

import java.math.BigDecimal;
import java.util.Collections;
import java.util.List;

import org.nasdanika.bank.ui.driver.pages.customer.AccountDetails;
import org.nasdanika.bank.ui.driver.pages.customer.Home;
import org.nasdanika.bank.ui.driver.pages.impl.NasdanikaBankPageImpl;
import org.openqa.selenium.WebDriver;

public class HomeImpl extends NasdanikaBankPageImpl implements Home {

	public HomeImpl(WebDriver webDriver) {
		super(webDriver);
	}

	@Override
	public org.nasdanika.bank.ui.driver.pages.guest.Home logOut() {
		return new org.nasdanika.bank.ui.driver.pages.impl.sketch.guest.HomeImpl(getWebDriver());
	}

	@Override
	public Home goHome() {
		return new HomeImpl(getWebDriver());
	}

	@Override
	public List<AccountSummary> getAccountSummaries() {		
		return Collections.singletonList(new AccountSummary() {
			
			@Override
			public AccountDetails navigateToDetails() {
				return new AccountDetailsImpl(getWebDriver());
			}
			
			@Override
			public String getLabel() {
				return null;
			}
			
			@Override
			public BigDecimal getBalance() {
				return null;
			}
			
		});
	}

}
```

### Account details

```java
package org.nasdanika.bank.ui.driver.pages.impl.sketch.customer;

import java.math.BigDecimal;
import java.util.Collections;
import java.util.Date;
import java.util.List;

import org.nasdanika.bank.ui.driver.pages.customer.AccountDetails;
import org.nasdanika.bank.ui.driver.pages.impl.NasdanikaBankPageImpl;
import org.openqa.selenium.WebDriver;

class AccountDetailsImpl extends NasdanikaBankPageImpl implements AccountDetails {

	AccountDetailsImpl(WebDriver webDriver) {
		super(webDriver);
	}

	@Override
	public org.nasdanika.bank.ui.driver.pages.guest.Home logOut() {
		return new org.nasdanika.bank.ui.driver.pages.impl.sketch.guest.HomeImpl(getWebDriver());
	}

	@Override
	public org.nasdanika.bank.ui.driver.pages.customer.Home goHome() {
		return new HomeImpl(getWebDriver());
	}

	@Override
	public List<Statement> getStatements() {
		return Collections.singletonList(new Statement() {

			@Override
			public String getLabel() {
				return null;
			}

			@Override
			public List<Transaction> select() {
				return Collections.singletonList(new Transaction() {

					@Override
					public Date getDate() {
						return null;
					}

					@Override
					public String getDescription() {
						return null;
					}

					@Override
					public BigDecimal getAmount() {
						return null;
					}

					@Override
					public BigDecimal getRunningBalance() {
						return null;
					}
					
				});
			}
			
		});
	}

}
```

## Summary

Now we have all the pieces in place to run our tests with the UI driver, let's do it!