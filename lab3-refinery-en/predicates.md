# Creating graph predicates in Refinery

The metamodel of the WebTest language does not enforce creating error-free models, so well-formedness constraints are needed. To check these constraints, we need to provide error patterns that indicate faulty models. [Refinery predicates](https://refinery.tools/learn/language/predicates/) can be used to define these error patterns. We can easily define [validation rules](https://refinery.tools/learn/language/predicates/#error-predicates) using these queries.

Refinery can also be used to examine the models of the WebTest language, find interesting models and model details that we can use in our program (e.g., code generators).


## Source file
In this task, you will work on the **webtest-refinery/webtest.refinery** problem file. This file contains several predicates that need to be implemented to check various well-formedness constraints on WebTest models, as well as to query interesting details from the models.

The _problem_ file containing the task is made up of different parts:
- **Metamodel**: this describes the the metamodel of the WebTest language. *This part should not be changed.*
- **Predicates**: here are the definitions of the predicates that can be fitted to the model. Initially, each predicate definition is a simple _false_; patterns never fit. *This is the part that needs to be changed during the lab.* The expected behaviour of the tasks is stated in a comment above the patterns.
- **Instance model**: Here you can write example models to test your solution, or you can copy one from the _instance-models_ folder. In the example models, The **Expected result** part describes where to expect the patterns to match for a correct implementation. The _declare_ part introduces the objects, the positive statements introduce types and edges (e.g. `Variable(variable1).`), and the other statements set the other values of the model to false. The model may contain weird parts on purpose, since we want to test the writing of a model that can detect these meaningless parts.


# Description of the tasks
In this exercise, the following steps are to be performed with each predicate:
- Read the specification commented above the predicate.
- Modify the definition of the predicate (after `<->`) to implement the desired specification. Do not change the name of the predicate or the parameter list!
- Check in the **Table** view the matching set of the predicates:
    * If there is no row marked with `error`, we agree with the solution. (Non-existent `::new` elements may show Error values, which is acceptable.)
    * Otherwise, a row marked with `error` indicates a mismatch between the expected behavior and the current implementation. Change the definition of the predicate until no `error` rows remain.

> [!IMPORTANT]  
> The example models may not cover all cases. It is your responsibility to ensure that your predicates work correctly for all possible models.  We will use additional models when verifying the solution.

A new predicate can be introduced, but not deleted. Please do not change the structure of the file! It is not necessary to use the **GENERATE** function in this part of the lab.

After the task is finished, save your solution (as `webtest.refinery`) and upload it to GitHub.

## Predicates to implement
You need to implement the following predicates in the **webtest-refinery/webtest.refinery** file:

### equalStringExpression(node e1, node e2)
Match StringExpression pairs with the same value. The StringExpression elements shall be different.

### equalElementExpression(node e1, node e2)
Match two different ElementExpression elements with the same tag and same label values.

### waitSecondsNotInteger(node w)
Match WaitStatement elements if their seconds attribute is not of Type INTEGER.

### longWait(node w)
Find WaitStatements where seconds > 10.
Note, that the IntegerExpression may be nested in one or more VariableExpressions.

### variableExpressionLoop(node e)
VariableExpression contains itself either directly or through one or more Variables and VariableExpressions.

### blockStatementWithThreeStatements(node s)
BlockStatement contains at least three statements.

### emptyName(node e)
Find elements with an empty name. A name is considered empty if it is set to an empty string.

### emptyVariableExpression(node e)
Match VariableExpressions that do not point to a variable.

### expressionType(node expression, node type)
Assign the correct Type to each Expression with the expressionType derived feature.
