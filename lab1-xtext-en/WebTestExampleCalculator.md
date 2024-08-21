# Example: calculator

Since finding the elements of a web page requires complex Selenium expressions, and we may need an element multiple times, the developers of Selenium suggest that we build an object model of a page and hides the details of finding the elements of the page. Using this page model we can write our tests at a higher abstraction level using the concepts of the web page itself.

For a conventional Selenium test we can write this page model in Java. The WebTest language makes creating a page model easy using the **page** keyword.

Let's take the following web page as an example: [The Online Calculator](https://www.theonlinecalculator.com/)

We can create the following object model for the web page:

```
page Calculator
  element display = input "display"
  element clear = input "AC"
  element add = input "+"
  element subtract = input "-"
  element multiply = input "×"
  element divide = input "/"
  element compute = input "="
  
  operation binaryOperation(string left, element op, string right)
    click clear
    fill display with left
    click op
    fill display with right
    click compute
  end
  
  operation multiply(string left, string right)
    binaryOperation using left,multiply,right
  end
end
```

In the example above the **page** keyword defines a web page named *Calculator*, in which we define the variables representing the HTML elements useful for us:

* display: the display of the calculator
* clear: the button that clears the display
* add: the addition operation
* subtract: the subtraction operation
* multiply: the multiplication operation
* divide: the division operation
* compute: the button that computes the result

After this we define a general binary operation called *binaryOperation* using the **operation** keyword with three parameters:

* left: the value of the left operand
* op: the operation to be performed between the operands
* right: the value of the right operand

The *binaryOperation* presses the *AC* button to clear the display. Then types the value of *left* into the display, presses the button represented by *op*, types the value of *right* into the display, and finally it pushes the *compute* button.

We also defined another operation called *multiply* that does a multiplication with the help of the *binaryOperation* operation.

In the WebTest language we can use the page model defined above as follows:

```
open "https://www.theonlinecalculator.com/"
context as Calculator
  wait until display exists
  print "Page opened"
  multiply using "23","6"
  assert display is "138"
end
```

This code opens the web page using the **open** keyword, then using the **context as** it assumes the web page conforms to the *Calculator* object model, i.e., the display and all the required buttons exist. The **wait** command waits until the *display* is visible. The **print** writes *Page opened* to the application log. Then the *multiply* operation is called with arguments *23* and *6*. The **assert** keyword checks whether the display contains the expected result *138*.
