# Construct the meta-model

## Task

During the construction of the meta-model, a model similar to a UML class diagram must be designed using object-oriented methods, which can represent all the elements of the WebTest language.

The design of this meta-model could be a task in the lab, however, for uniformity and testability, we give you the graphical representation of the [meta-model](MetaModel.md) designed by us. You have to map this graphical representation into Xcore. 

You have to work in the **model/webtest.xcore** file inside the **webtest.model** project.

## Check the solution

The **webtest.model.tests** project tests whether the Xcore meta-model is correct according to the class diagrams. The project contains the following test classes:

* **MetaModelTests**: checks the common part of the meta-model
* **CaptureTests**: checks the **Capture** extension
* **ForEachTests**: checks the **ForEach** extension
* **JavaScriptTests**: checks the **JavaScript** extension
* **ManualTests**: checks the **Manual** extension
* **TestParamsTests**: checks the **TestParams** extension

You only have to keep the classes relevant to you. You can make a comment of the inner contents of the rest of the test classes.

The **MetaModelTests** class and its contents must be preserved intact.

## To be uploaded

During the solution of the task, take screenshots taken from the following parts, and upload them into the folder **homeworks/hw1** of your own git repo:

* The Xcore representation of the **IfStatement**.
* The Xcore representation of the extensions assigned to you.
* The test summary window in Eclipse for the **webtest.model.tests** project, which shows that all the relevant test are executed successfully.

