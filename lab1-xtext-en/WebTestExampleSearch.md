# Example: DuckDuckGo search

The WebTest language contains various statements that can be used to describe the execution of test cases.

An example for a simple DuckDuckGo search in the WebTest language:
```
webtest example.Search

open "https://duckduckgo.com/"
fill input "q" with "jwst"
click button "Search"
wait 5 seconds
```

The **webtest** keyword defines a Java class that contains the JUnit test. In this case it will be the *example.Google* class.

The **open** keyword opens the URL *https://duckduckgo.com/*.

The **fill** keyword types *jwst* into the input identified by *q*.

The **click** keyword presses the *Search* button.

The **wait** keyword waits for 5 seconds so that we can look at the search results.

The example above could be translated into Java code that uses Selenium to control a Chrome browser to perform the appropriate statements:

```Java
var driver = new ChromeDriver();
driver.manage().window().maximize();

var wait = new FluentWait<WebDriver>(driver);
wait.pollingEvery(Duration.ofSeconds(1));
wait.withTimeout(Duration.ofSeconds(10));

// open "https://duckduckgo.com/"
driver.navigate().to("https://duckduckgo.com/");

// fill input "q" with "jwst"
var inputQ = By.xpath("//input[@name='q']");
wait.until(ExpectedConditions.presenceOfElementLocated(inputQ));
driver.findElement(inputQ).sendKeys("jwst");

// click button "Search"
var buttonSearch = By.xpath("//button[contains(text(),'Search')]");
wait.until(ExpectedConditions.presenceOfElementLocated(buttonSearch));
driver.findElement(buttonSearch).click();

// wait 5 seconds
Thread.wait(5000);

driver.quit();
```

The Selenium code opens a Chrome browser and makes it full screen. Since the loading of a web page may take some time, we may have to wait for some elements to appear. The *FluentWait* class helps with this: we set the maximum wait time to 10 seconds and that the conditions should be checked in every second. Then we open the page *https://duckduckgo.com/*, and wait for the search field to be displayed. Then we type *jwst* into the search field, push the *Search* button and wait for 5 seconds to see the search results.

The elements of a web page (buttons, input fields, etc.) can be accessed using the *findElement* function of the *WebDriver* class. The function needs a search condition which can be specified using the *By* class. There are many possible search criteria. Details can be found in the [Selenium](https://www.selenium.dev/documentation/webdriver/elements/locators/) documentation.

