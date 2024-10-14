# Creating the VIATRA queries
Model queries may be needed for several reasons. The WebTest language's metamodel does not enforce creating error-free models, so well-formedness constraints are needed. To check these constraints, we need to provide error patterns that indicate faulty models. VIATRA queries are used to write these error patterns. Later, we can easily derive validation rules from these queries ([see](https://static.incquerylabs.com/projects/viatra/viatra-docs/ViatraDocs.html#_validation)), which we will not use in the homework task.

VIATRA can also be used to examine the WebTest language's models, find interesting models and model details that we can use in our program (e.g., code generators).

Finally, VIATRA queries can be used to specify the preconditions of model transformations. (This will be covered in the next task.)

In the **webtest.transformation** project, open the **queries.vql** file in the **webtest.transformation.queries** package within the **queries** folder, and create queries for the well-formedness constraints and other queries defined below!

It is important that the names and parameters of the queries match the names and parameters defined below. The queries have varying difficulty (between 1 and 10 lines), and are not introduced in order of difficulty.

## Constraints
### variableSameElement(var1 : Variable, var2 : Variable)
Two Variables contain ElementExpressions whose _tag_ values are the same and _label_ values are equal.

### variableSameString(var1 : Variable, var2 : Variable)
Two Variables contain StringExpressions whose _value_ attributes are the same.

### variableExpressionLoop(e : VariableExpression)
A VariableExpression indirectly contains itself through one or more Variables (and VariableExpressions).

### waitSecondsNotInteger(wait : WaitStatement)
The _seconds_ property of the WaitStatement is not of type Type.INTEGER.

### waitConditionNotBoolean(wait : WaitStatement)
The _condition_ property of the WaitStatement is not of type Type.BOOLEAN.

## General queries
### twoDeepVariableExpression(e : VariableExpression)
A VariableExpression contains at least two VariableExpressions nested within each other.

### tenDeepvariableExpression(e : VariableExpression)
Exactly ten VariableExpressions are nested within a VariableExpression.

###  operationWithThreeParameters(o : Operation)
An Operation contains at least three parameters.

