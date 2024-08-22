# Laboratory 2 - IDE support and Xtend generator

## Steps to solve the lab

0. In this lab you have to develop the application from the previous lab further.
1. Solve the [Xtend](https://eclipse.dev/Xtext/documentation/103_domainmodelnextsteps.html) tutorial!
2. Solve the tasks listed below, while also taking the extensions assigned to you into account!
4. Before submission, check whether the `compile.bat` command in **webtest.dsl.parent** runs successfully. All projects must compile without any errors.
5. Add the **hw2** git tag to your last commit!

## Goal of the lab

The goal of this lab is to add a more advanced Eclipse IDE support to the WebTest language, and to develop a code generator that translates the WebTest code into JUnit tests, so that they can be executed.

The common part of the language must be realized by everyone. From the extensions, you only have to implement the two extensions assigned to you. Of course, you can implement the other extensions, too, if you want to.

## List of the tasks

In this lab you have to develop the application from the previous lab further. In Eclipse, open the projects under the folder **webtest.dsl.parent**.

IMPORTANT: Within the same project, files with extension **xtend** see files with extension **java** only inside the Eclipse IDE, but not during the Maven build! Therefore, it is recommended to use **xtend** files only in the **webtest.generator** project. Always make sure that the Maven build is successfull, too!

Solve the following tasks:

1. [Outline view](TaskOutline.md) (5 points)
2. [Syntax highligthing](TaskHighlighting.md) (5 points)
3. [Project wizard](TaskProjectWizard.md)
4. [Code generation](TaskCodeGeneration.md) (15 points)

## Check the solution

The **webtest.dsl.ui.tests** project tests whether the Xtext IDE support is correct.

The solution of the lab tasks can be regarded as complete, if all the relevant tests run successfully, and the Maven compilation is also successful using the `compile.bat` command. However, during the evaluation of your solution, we may execute further tests, too.

When you are finished with all the tasks, the following behavior is expected:

1. Start the **Runtime Eclipse**
2. Create a new **WebTest project** using the wizard
3. Add a new WebTest file with a **.wt** extension to the **webtest** folder
4. Write some tests inside the newly created file using the WebTest syntax, while enjoying the IDE support for the language: syntax highlighting, outline, validation, etc.
5. Save the WebTest file. The corresponding JUnit test must be generated automatically.
6. Right click on the project, and select **Run As > JUnit Test**. The JUnit test should execute successfully while controlling a browser through the Selenium library and performing the steps written in the WebTest file.

## References

Useful links to solve the tasks:

* [Xtext](https://eclipse.dev/Xtext/documentation/index.html)
* [Xtend](https://eclipse.dev/Xtext/xtend/documentation/index.html)
* [Guidelines](../lab1-xtext/images/PR3-Xtext-Guideline.pdf) from the previous semester's Xtext practice

