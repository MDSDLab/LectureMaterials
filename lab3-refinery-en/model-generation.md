# Model generation in Refinery

In the previous exercise, you created graph predicates to check various well-formedness constraints on models of the WebTest language. In this exercise, you will use these predicates along with new [error predicates](https://refinery.tools/learn/language/predicates/#error-predicates) to generate test models for the WebTest language using the model generation feature of Refinery. You will modify an [instance model](https://refinery.tools/learn/language/logic/) to allow the generator to extend the model and create valid test cases that satisfy the well-formedness constraints specified by the predicates.

## Source file
In this task, you will work on the **webtest-refinery/generation.problem** _problem_ file. This file contains an instance model of the WebTest language. You will use this model to generate test cases for the WebTest language using the predicates defined in the previous exercise. The problem file containing the task is made up of different parts:
- **Metamodel**: this describes the the metamodel of the WebTest language. *This part should not be changed.*
- **Predicates**: here, you can define error predicates that can be used to guide the generation of the models. *You can add new predicates here if needed for the generation tasks.*
- **Instance model**: Here is the definition of the instance model. *You need to modify this part to generate the required test models.*

## Description of the task

In this task, you will generate test models for the WebTest language. You may use the predicates defined in the previous exercise to guide the generation of the models (copy and paste the one you would like to use). You may also define new (error) predicates. You will need to modify the instance model to allow the generator to extend the model and create valid test cases that satisfy the well-formedness constraints specified by the predicates. You may also add type scope constraints to limit the size of the generated models.

We defined error predicates for two of the requirements below, which you need to implement.

You need to modify the problem file to generate test models with the following characteristics:

- The initial model shall only be modified and extended at places necessary to satisfy the constraints.
- Each `BlockStatement` shall contain at least one `statement` (implement the `emptyBlockStatement` error predicate).
- Make sure that any `BlockStatement` that do not contain any `statement` elements in the initial model can be completed by any type of `Statement` or `Expression` by the generator. Allow the generator to create new elements here.
- Each `IfStatement` shall contain a `then` block (implement the `emptyIfStatement` error predicate).
- If an `IfStatement` does not contain a `then` block in the initial model, manually create one, and allow the generator to complete it with any type of  `Statement` or `Expression`. Allow the generator to create new elements here.
- The following predicates defined in the previous exercise shall not have any matches in the generated models:
    - waitSecondsNotInteger
    - variableExpressionLoop
    - blockStatementWithThreeStatements
- The generator shall add at least 1 and at most 4 new BlockStatement elements to the model.
- The generated models should have at least 40 and at most 50 elements in total.
- The attribute values of the added elements do not need to be set.

After the task is finished, save your solution (as `generation.problem`) and upload it to github.

Finally, generate at least 5 models by clicking the **Generate** button. Save the generated models to your local machine (click the **Export** button in the top right corner of the _Graph view_, select **Refinery** as the file type, and click **Download**) and upload them to your repository in the `generated-models` folder.