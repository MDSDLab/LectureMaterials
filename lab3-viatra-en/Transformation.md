<!-- Translate to English -->
# Creating the transformation rules

In the **webtest.transformation** project, open the **queries.vql** file in the **webtest.transformation.queries** package within the **queries** folder, and create the preconditions of the transformation rules defined below.

In the **webtest.transformation** project, open the **MutationRules.xtend** file in the **webtest.transformation.rules** package within the **src** folder, and create the transformation rules defined below! It is important that the names of the queries associated with the rules match the names provided in parentheses!

## Remove WaitStatement from BlockStatement (removeWaitStatement)
Find a BlockStatement that contains a WaitStatement, and remove the WaitStatement from it!

## Set the timeout value of the WaitStatement (setSeconds)
Find a WaitStatement that does not have a _seconds_ timeout value, and set a 1-second timeout!

## Modify the timeout value of the WaitStatement (modifySeconds)
Find a WaitStatement whose _seconds_ timeout value is greater than 10, and make the _seconds_ value half of the original! (If the new _seconds_ value is not an integer, round it down!)

Take into account that the _seconds_ value may not only be an IntegerExpression, but can be wrapped in a VariableExpression multiple times! In such cases, do not change the structure, just change the value of the wrapped IntegerExpression!

## Remove PrintStatement from BlockStatement (removePrintStatement)
Find a BlockStatement that contains a PrintStatement, and remove the PrintStatement from it!

## Modify the URL (modifyURL)
Find a StringExpression that contains the character sequence "https://" anywhere in its value, and replace the character sequence "https://" with "http://"!

## Modify the URL 2 (modifyURL2)
Find a StringExpression that starts with the character sequence "https://www.", and remove the "https://www." character sequence from the beginning of the String!

