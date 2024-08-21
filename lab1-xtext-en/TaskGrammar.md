# Develop the grammar

## Task

In order to process the textual representation of the WebTest language, we need a compiler. To construct the compiler, we have to have a formal description of this textual representation. The Xtext grammar serves this purpose. Detailed description about Xtext can be found [here](https://eclipse.dev/Xtext/documentation/301_grammarlanguage.html).

Inside the **webtest.dsl** project under the folder **src** from package **webtest.dsl** open the **WebTestDsl.xtext** file, and write the grammar for the Xtext language!

It is important to note, that the Xcore meta-model cannot be mechanically mapped to grammar rules. It is the other way around: grammar rules must be mapped to Xcore elements. Multiple grammar rules can result in similar Xcore objects, there is no one-to-one mapping between the two representations. The grammar must be constructed based on the language specification of the WebTest language. The grammar must be able to process all the files with **.wt** extensions inside the **webtest.example** project, too.

To help you start with the solution, we give you some of the grammar rules:

```
grammar webtest.dsl.WebTestDsl with org.eclipse.xtext.common.Terminals

import "webtest.model"

Main: 'webtest' testClass+=ID ('.' testClass+=ID)* declarations+=Declaration* body=BlockStatement;

// ...

Expression: NotExpression;
NotExpression returns Expression: {NotExpression} 'not' operand=ConditionalExpression | ConditionalExpression;
ConditionalExpression returns Expression: IsExpression | ContainsExpression | ExistsExpression | SimpleExpression;
IsExpression: left=ReferenceExpression 'is' right=SimpleExpression;
ContainsExpression: left=ReferenceExpression 'contains' right=SimpleExpression;
ExistsExpression: operand=ReferenceExpression 'exists';
SimpleExpression returns Expression: ReferenceExpression | StringExpression | IntegerExpression | BooleanExpression;
ReferenceExpression returns SimpleExpression: ElementExpression | VariableExpression;
ElementExpression: tag=ID label=STRING;
VariableExpression: variable=[Variable];
StringExpression: value=STRING;
IntegerExpression: value=INT;
BooleanExpression: value?='true' | {BooleanExpression} 'false';
```

The preparation of the other rules is the task of this lab. The common part of the WebTest language must be realized by everyone. From the extensions, you only have to implement the two extensions assigned to you. Of course, you can implement the other extensions, too, if you want to.

Some guidelines for the solution:

* The name of a grammar rule must be the same as the name of the Xcore class it maps to. If the name is different, you can specify the Xcore class using the `returns` keyword.
* A single grammar rule can instantiate a single Xcore object, and the instantiation of an Xcore object cannot be split into multiple rules.
* If a rule does not necessarily result in an Xcore object (e.g., none of the properties are assigned), but you still want to create an instance, you have to specify the Xcore class name inside curly braces (`{...}`).
* The Xcore properties can be assigned using the `=`, `+=` and `?=` operators. The property name in the grammar must be the same as the property name in the Xcore class.
* The presence of optional elements can be evaluated using the operator `?=`.
* The operator `+=` can be used to add elements to collection properties.
* You can refer to other objects using the `[...]` syntax. Between square brackets you have to specify the type of the object you want to refer to. The reference is resolved by Xtext using the name of the object. If the default resolution strategy is insufficient, you can override it later (see later: scoping in name analysis)
* In order to be able to reference an object, it must have a `name` property, through which the name resolution happens. The Xcore meta-model was designed taking this into account.

After you are finished with your grammar rules, or after any time you change these rules, you must execute the **GenerateWebTestDsl.mwe2** generator inside the **webtest.dsl** project under the **src** folder in package **webtest.dsl**. Right click on the generator and choose **Run as > MWE2 Workflow**!

It may happen that the execution of the workflow indicates some errors in your grammar (e.g. left recursion or multiple alternatives match the same pattern). If there are errors in your grammar, you must fix them! Otherwise, the generated files will be incorrect. With a wrong grammar, all the following parts of the laboratories can go wrong!

After the generation is successful, right click on the **webtest.dsl** project, and select **Run as > Eclipse application**! This will open another Eclipse called **Runtime Eclipse**, in which you can edit WebTest files having an extension **.wt**. In this Runtime Eclipse you can try the current state of your compiler.

## Check the solution

You can check your grammar using the **webtest.dsl.tests** project, by running it as a JUnit test (**Run as > JUnit Test**). The classes **ParseTests** and **ParseExtensionsTests** check the correctness of the grammar.

## To be uploaded

During the solution of the task, take screen shots taken from the following parts, and upload them into the folder **homeworks/hw1** of your own git repo:

* A **.wt** file of at least 20 lines long opened in **Runtime Eclipse**. The file should contain examples to at least 5 common language elements and the 2 extensions assigned to you.
* The **WebTest.xtext** file opened in **Eclipse** showing the grammar rules for the 2 extensions assigned to you.
* The test summary window in **Eclipse** for the **webtest.model.tests** project, which shows that all the tests in **ParseTests** and **ParseExtensionsTests** are executed successfully.
