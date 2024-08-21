# Laboratory 1 - Xcore meta-model, Xtext grammar, name analysis, type analysis

## Steps to solve the lab

0. Download and install the following software:

  * [Eclipse IDE for Java and DSL Developers 2024-06 R](https://www.eclipse.org/downloads/packages/release/2024-06/r/eclipse-ide-java-and-dsl-developers). IMPORTANT: use exactly this version and this edition of Eclipse, since the projects only compile with this!
  * [Maven](https://maven.apache.org/download.cgi). Add the `bin` folder of Maven to the `PATH` environment variable.
  * JDK 21 (e.g., [Adoptium](https://adoptium.net/)). Make sure that the `JAVA_HOME` environment variable points to the main folder of the installed JDK.

1. Solve the tutorials for [Xcore](https://wiki.eclipse.org/Xcore) and [Xtext](https://eclipse.dev/Xtext/documentation/102_domainmodelwalkthrough.html)!
2. From the [mdsd-2024-lab1-xtext](https://github.com/MDSDLab/mdsd-2024-lab1-xtext) repository copy the folder **webtest.dsl.parent** to the root of your own repository!
3. Solve the tasks listed below, while also taking the [extensions assigned to you](ExtrasTable2024.md) into account!
4. Before submission, check whether the `compile.bat` command in **webtest.dsl.parent** runs successfully. All projects must compile without any errors.
5. Add the **hw1** git tag to your last commit!

## Goal of the lab

The goal of this lab is to construct the meta-model of the [WebTest language](WebTestLanguageSpecification.md), and to implement the compiler of the language including name- and type analysis.

The [common part](WebTestReference.md) of the language must be realized by everyone. From the [extensions](WebTestReferenceExtra.md), you only have to implement the two extensions assigned to you. Of course, you can implement the other extensions, too, if you want to.

## List of the tasks

In Eclipse, open the projects under the folder **webtest.dsl.parent** using **File > Import... / General > Existing Projects into Workspace** (not Maven > Existing Maven Projects!). Select the **webtest.dsl.parent** folder as **root directory**! After opening the projects, there may be compilation errors in the **webtest.dsl.tests** and **webtest.model.tests** projects. These errors are present, because the lab tasks are not finished yet. After completing the tasks, these errors should disappear. The other projects must load without errors.

In the **webtest.dsl** project under the **src** folder in the **webtest.dsl** package you can find the class **WebTestExtensions**. In this class, set the flags to **false** for those extensions which were not assigned to you. This way, only the tests relevant for you are executed.

IMPORTANT: Within the same project, files with extension **xtend** see files with extension **java** only inside the Eclipse IDE, but not during the Maven build! Therefore, it is recommended to omit **xtend** files in this laboratory. In case you intend to use them, make sure that the Maven build is successfull with them, too!

Solve the following tasks:

0. [Try the example](TaskExample.md)
1. [Construct the meta-model](TaskMetaModel.md) (5 points)
2. [Develop the grammar](TaskGrammar.md) (10 points)
3. [Name analysis](TaskNameAnalysis.md) (5 points)
4. [Type analysis](TaskTypeAnalysis.md) (5 points)

## Check the solution

The **webtest.model.tests** project tests whether the Xcore meta-model is correct.

The **webtest.dsl.tests** project tests whether the Xtext grammar, and the name- and type analysis are correct.

The solution of the lab tasks can be regarded as complete, if all the relevant tests run successfully, and the Maven compilation is also successful using the `compile.bat` command. However, during the evaluation of your solution, we may execute further tests, too.

## References

Useful links to solve the tasks:

* [Xcore Wiki](https://wiki.eclipse.org/Xcore)
* [Xtext](https://eclipse.dev/Xtext/documentation/index.html)
* [Xtend](https://eclipse.dev/Xtext/xtend/documentation/index.html)
* [Guidelines](../lab1-xtext/images/PR3-Xtext-Guideline.pdf) from the previous semester's Xtext practice
* Inside Xcore operations you can use Xbase expressions. The Xtend language also uses Xbase expressions, so the best reference for Xbase is the following: [Xtend Expressions](https://eclipse.dev/Xtext/xtend/documentation/203_xtend_expressions.html)

