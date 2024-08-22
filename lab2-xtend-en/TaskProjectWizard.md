# Project wizard

In this task you don't have to do anything, just read the text below.

To be able to execute the JUnit tests generated from the WebTest files, they have to be embedded in a proper project (see the **webtest.example** project).

To help you, we have added a wizard (**src/webtest/dsl/ui/wizard/WebTestProject.java**) to the **webtest.dsl.ui** project which constructs the required Maven-based Eclipse project for you:

* **src/main/java**: this folder contains Java source files in a Maven project
  * **.gitignore**: configuration file for git (it's empty, it's goal is to preserve the **src/main/java** folder under version control)
* **src/main/resources**: this folder contains configuration files in a Maven project
  * **logback.xml**: configuration file for the slf4j logging system
* **src/test/java**: this folder contains test Java source files in a Maven project
  * **.gitignore**: configuration file for git (it's empty, it's goal is to preserve the **src/test/java** folder under version control)
* **src/test/resources**: this folder contains test configuration files in a Maven project
  * **logback-test.xml**: configuration file for the slf4j logging system
* **webtest**: the WebTest files with **.wt** extension must be placed in here
  * **HelloWorld.wt**: a Hello World sample code with WebTest syntax
* **.classpath**: configuration file for Eclipse that determines the source folders
* **.gitignore**: configuration file for git (omits the **target** and **output** folders from version control)
* **pom.xml**: configuration file for Maven, which contains the JUnit 5 and Selenium dependencies
* **src/test/java/webtest/selenium/api/Page.java**: base class for web page object models
* **src/test/java/webtest/selenium/api/PageElement.java**: represents an element on a web page
* **src/test/java/webtest/selenium/api/SeleniumTest.java**: base class for JUnit tests using Selenium with a lot of utility methods

The contents of the files above are produced by the **webtest.generator.WebTestProjectGenerator** class in the **webtest.generator** project using Xtend templates.

The JUnit tests generated from the WebTest files found in the **webtest** folder are put into the **src-gen/test/java** folder. This step is however not performed by the wizard, it must be performed in the [next task](TaskCodeGeneration.md) by your generator.

Don't modify the wizard or any of the related files! (Except maybe the value of the **DEFAULT_CHROME_DRIVER_LOCATION** constant in the generated **SeleniumTest.java** class, if necessary.)

## To be uploaded

Since there is nothing to do in this task, you don't have to upload any screenshots.
