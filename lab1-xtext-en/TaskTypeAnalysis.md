# Type analysis

The goal of type analysis is to determine the types of expressions and check whether these types are compatible with the expected type at that point in the code.

Xtext performs no type analysis. We have to do this ourselves.

In addition, we have to report errors if anything goes wrong during type analysis. This can be done using  [validation](https://eclipse.dev/Xtext/documentation/303_runtime_concepts.html#validation).

## Modification of the Xcore meta-model

Inside the **webtest.model** project under the **model** folder open the **WebTest.xcore** file, and modify the `Type` enumeration as follows:

```Java
enum Type
{
    UNDEFINED,
    ERROR,
    ANY,
    STRING,
    INTEGER,
    BOOLEAN,
    ELEMENT
}
```

We have two additional values compared to the ones before:

* `ERROR`: marks that the type of a given expression or variable could not be determined
* `ANY`: marks that at some point in the code any type of expression is acceptable (e.g., the arguments of the `print` statement)

Modify the `Expression` class as follows:
```Java
abstract class Expression
{
    Type expectedType
    Type actualType

    op void computeTypes() {}
}
```

The `expectedType` property stores the expected type of the expression, while the `actualType` property stores the actual type. If you remember from previous semester: `expectedType` is an inherited attribute which has to be computed top-down, while `actualType` is a synthesized attribute which has to be computed bottom-up. These computations can be performed by overriding the `computeTypes()` operation inside the descendants of the `Expression` class: the actual type of the current expression must be computed and stored, and the expected types of all the directly contained sub-expressions must be set.

Modify the `Main` class as follows:
```Java
class Main
{
    unsettable boolean typesComputed
    String[] testClass
    contains Declaration[] declarations
    contains BlockStatement body
    
    op void computeTypes() {
        if (typesComputed) return;
        typesComputed = true
        declarations.forEach[it.computeTypes()]
        body?.computeTypes()
    }
}
```

The `computeTypes()` operation of `Main` can be used to start the type computation for the whole syntax tree.The `typesComputed` property helps to avoid running the computation multiple times.

Introduce the `computeTypes()` operation in all the other classes ( `Page`, `Operation`, `Statement` etc.).

The body of the `computeTypes()` operation should follow the following template to get the correct evaluation order of the `actualType` and `expectedType` attributes:

```Java
class X
{
    contains Y y

    op void computeTypes() {
        this.actualType = Type.<type>     // if X is an expression
        if (y !== null) {
            y.expectedType = Type.<type>  // if Y is an expression
            y.computeTypes()
        }
    }
}
```

For example:

```Java
class IsExpression extends BinaryExpression
{
    op void computeTypes() {
        actualType = Type.BOOLEAN
        if (left !== null) {
            left.expectedType = Type.ELEMENT
            left.computeTypes()
        }
        if (right !== null) {
            right.expectedType = Type.STRING
            right.computeTypes()
        }
    }
}
```

Finally, by calling `computeTypes()` on `Main` we can perform type computation for the whole syntax tree.

## Actual and expected types of expressions

The following list specifies the actual types of expressions and the expected types of their sub-expressions:

* `<elem> is <value>`: the expected type of `elem` is `ELEMENT`, the expected type of `value` is `STRING`, the actual type of the whole expression is `BOOLEAN`
* `<elem> contains <value>`: the expected type of `elem` is `ELEMENT`, the expected type of `value` is `STRING`, the actual type of the whole expression is `BOOLEAN`
* `<elem> exists`: the expected type of `elem` is `ELEMENT`, the actual type of the whole expression is `BOOLEAN`
* `not <value>`: the expected type of `value` is `BOOLEAN`, the actual type of the whole expression is `BOOLEAN`
* string constant: the actual type is `STRING`
* integer constant: the actual type is `INTEGER`
* boolean constant: the actual type is `BOOLEAN`
* HTML element constant: the actual type is `ELEMENT`
* variable reference as an expression: the actual type of the expression is the type of the variable

The following list specifies the expected types of expressions inside statements:

* `<type> <name> = <value>`: the expected type of `value` is `type`, the type of the variable `name` is `type`
* `if <condition> then ... else ... end`: the expected type of `condition` is `BOOLEAN`
* `while <condition> do ... end`: the expected type of `condition` is `BOOLEAN`
* `<operation name> using <value>...`: the expected types of the `value` arguments are the actual types of the corresponding parameters
* `open <url>`: the expected type of `url` is `STRING`
* `fill <elem> with <text>`: the expected type of `elem` is `ELEMENT`, the expected type of `text` is `STRING`
* `click <elem>`: the expected type of `elem` is `ELEMENT`
* `context <elem> as <page> ... end`: the expected type of `elem` is `ELEMENT`
* `print <value>...`: the expected type of the `value` arguments is `ANY`
* `assert <condition>`: the expected type of `condition` is `BOOLEAN`
* `wait <time> seconds until <condition>`: the expected type of `time` is `INTEGER`, the expected type of `condition` is `BOOLEAN`

The following list specifies the expected types of expressions inside extension statements:

* `capture <elem>`: the expected type of `elem` is `ELEMENT`
* `javascript <code> using <value>...`: the expected type of `code` is `STRING`, the expected type of the `value` arguments is `ANY`
* `foreach <item> in <items>`: the type of `item` is `ELEMENT`, the expected type of `items` is `ELEMENT`
* `test <name>(<parameter>...) with <value>... <statement>... end`: the expected types of the `value` arguments are the actual types of the corresponding parameters

## Validation

Inside the **webtest.dsl** project under the **src** in the **webtest.dsl.validation** package create a **TypeValidator.java** file similarly to **NameValidator.java**.

Inside the **webtest.dsl** project under the **src** from the **webtest.dsl.scoping** package open the **WebTestDslValidator.java** file, and register the **TypeValidator** class as follows:

```Java
package webtest.dsl.validation;

import org.eclipse.xtext.validation.ComposedChecks;

/**
 * This class contains custom validation rules. 
 *
 * See https://www.eclipse.org/Xtext/documentation/303_runtime_concepts.html#validation
 */
@ComposedChecks(validators = {NameValidator.class, TypeValidator.class})
public class WebTestDslValidator extends AbstractWebTestDslValidator {
	
}

```

Make the following validation inside the **TypeValidator** class:

* Report if the actual type is incompatible with the expected type: `"Expression of type <expected type> expected but <actual type> was found."`. The possible type names to be substituted are: `STRING`, `INTEGER`, `BOOLEAN`, `ELEMENT` or `UNDEFINED`.


## Check the solution

Errors reported by the validators are marked by red underscores inside **.wt** files in **Runtime Eclipse**.

You can check your name analysis implementation using the **webtest.dsl.tests** project, by running it as a JUnit test (**Run as > JUnit Test**). The classes **TypeAnalysisTests** and **TypeAnalysisExtensionsTests** check the correctness of the name analysis.


## To be uploaded

During the solution of the task, take screen shots taken from the following parts, and upload them into the folder **homeworks/hw1** of your own git repo:

* The test summary window in **Eclipse** for the **webtest.model.tests** project, which shows that all the tests in **TypeAnalysisTests** and **TypeAnalysisExtensionsTests** are executed successfully.
