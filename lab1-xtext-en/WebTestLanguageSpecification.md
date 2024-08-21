# WebTest language specification

The goal of the WebTest language is to simplify the testing of web pages.

Using the language we can describe various scenarios at a high abstrafction level based on commonly used steps (e.g., open a given URL, type something to an input field, click a button).

From the high abstraction level description the WebTest compiler automatically generates [JUnit 5](https://junit.org/junit5/docs/current/user-guide/) tests which build on the [Selenium](https://www.selenium.dev/documentation/) library to control a Chrome browser to perform the steps written in the WebTest language.

The following pages provide more details about the WebTest language:

* [Example: Google search](WebTestExampleGoogle.md)
* [Example: calculator](WebTestExampleCalculator.md)
* [Example: wizard](WebTestExampleWizard.md)
* [Example: dialog boxes](WebTestExampleDialog.md)
* [WebTest language reference](WebTestReference.md)
* [WebTest language extensions](WebTestReferenceExtra.md)
