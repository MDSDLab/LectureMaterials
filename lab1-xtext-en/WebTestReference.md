# WebTest language reference

## WebTest file structure

WebTest files have the **.wt** extension. The structure of a WebTest file is the following:

```
webtest <package>.<class>

(<page> | <test> | <operation>) ...

<statement>...
```

At the beginning of the file the **webtest** keyword defines the Java class (&lt;class>) with full package prefix (&lt;package>) that will result in a JUnit test by compiling the WebTest file. After this, any number of web page model (&lt;page>), test case (&lt;test>) and operation (&lt;operation>) may follow in any order. Finally any number of statements (&lt;statement>) may come.

An example for such a file structure:

```
webtest example.Calculator

page Calculator
  // ...
end

test add3and6
  // ...
end

test multiply23and6
  // ...
end
```

Another example:

```
webtest example.Search

operation search(string text)
  fill input "q" with text
  click button "Search"
end

open "https://duckduckgo.com/"
search using "jwst"
```

## Web page object model (page ... end)

The WebTest language makes modeling a whole web page or just a part of a page (e.g., a form or a dialog box) easier using the **page** keyword.

The structure of a page object model is the following:

```
page <name>
  <variable>...
  <operation>...
end
```

The object model must have a name (&lt;name>), and in its body we can define variables (&lt;variable>), which can represent HTML elements or other (string, integer, etc.) values. In addition, we can define operations  (&lt;operation>), which perform a series of statements.

### Variables representing HTML elements

A variable representing an HTML element can be defined as:

```
element <name> = <tag> <label>
```

A variable must have a name (&lt;name>), an HTML tag (&lt;tag>) kind and a label (&lt;label>). The tag and the label help to identify the exact HTML element the variable refers to. The details of the HTML element identification are described in the expressions section.

### operation ... end

The structure of an operation is the following:

```
operation <name>(<parameter>...)
  <statement>...
end
```

An operation must have a name (&lt;name>), which is optionally followed by a list of comma separated parameters (&lt;parameter>) enclosed in parenthesis. The body of an operation contains a list of statements which can use variables, parameters and call other operations.

An example for a web page object model:

```
page Calculator
  element display = input "number display"
  element clear = button "AC"
  element add = button "+"
  element subtract = button "-"
  element multiply = button "×"
  element divide = button "/"
  element compute = button "="
  
  operation binaryOperation(text left, element op, text right)
    click clear
    fill display with left
    click op
    fill display with right
    click compute
  end
  
  operation multiply(text left, text right)
    binaryOperation using left, multiply, right
  end

  operation clear
    click clear
  end
end
```

## Test case (test ... end)

The structure of a test case is the following:

```
test <name>
  <statement>...
end
```

A test case must have a name (&lt;name>), and its body contains a list of statements. Test cases are executed independently of each other in any order.

An example for a test case:

```
test login
  fill input "username" with "alice"
  fill input "password" with "secret"
  click button "Sign in"
  assert label "Signed in as: Alice" exists
  assert button "Sign out" exists
end
```

## Statements

### Variable definition

A variable is defined by the following syntax:

```
<type> <name> = <value>
```

A variable must have a type (&lt;type>), a name (&lt;name>) and a value (&lt;value>). The value cannot be modified: variables can get their values only at the place of their definition. The type must be one of the following:

* **string**: represents a string of characters
* **integer**: represents an integer number
* **boolean**: represents a logical true or false value
* **element**: represents an HTML element

Examples:

```
string url = "https://duckduckgo.com/"
integer timeout = 10
element logout = button "Sign out"
boolean loggedIn = logout exists
```

### if ... then ... else ... end

The **if** statement defines conditional execution:

```
if <condition:boolean> then
  <statement>...
else
  <statement>...
end
```

After the **if** keyword a condition (an expression of **boolean** type)  must be defined followed by the **then** keyword and the statements which should be executed when the condition is true. Statements after the **else** keyword are executed when the condition is false. The **else** branch is optional.

Examples:

```
if not loggedIn then
  fill username with "alice"
  fill password with "secret"
  click login
end

if homeButton exists then
  click homeButton
else
  print "Error"
end
```

### while ... do ... end

The **while** statement defines a loop:

```
while <condition:boolean> do
  <statement>...
end
```

After the **while** keyword a condition (an expression of type **boolean**) must be defined followed by the **do** keyword and the statements, which should be executed as long as the condition is true.

Example:

```
element nextButton = button "next"
element finishButton = button "finish"

while nextButton exists do
  click nextButton
end
click finishButton
```

### calling an operation

When calling an operation, we have to refer to the name of the target operation followed by the **using** keyword and the same number of arguments (expressions) as many parameters the target operation has. If the operation has no parameters, the **using** keyword must be omitted. The arguments are separated by commas, and they are assigned to the parameters in the order the parameters are defined:

```
<operation name> using <value>...
```

Examples:

```
operation login(string username, string password)
  fill input "username" with username
  fill input "password" with password
  click button "Sign in"
end

operation logout
  click button "Sign out"
end

login using "alice", "secret"
logout
```

In the example above, the value *"alice"* is assigned to the *username*, the value *"secret"* is assigned to the *password* parameter.

### open

The **open** statement expects a **string**-typed URL, and opens it in the browser:

```
open <url:string>
```

Examples:

```
open "https://duckduckgo.com/"

string github = "https://github.com"
open github
```

### fill ... with ...

The **fill** statement types a string of characters into the referenced HTML element:

```
fill <elem:element> with <text:string>
```

The **fill** statement expects an element (&lt;elem>) of type **element**, and a string of characters (&lt;text>) of type **string**.

Examples:

```
fill input "username" with "alice"

element password = input "password"
string secret = "secret"
fill password with secret
```

### click ...

The **click** statement clicks the referenced element with the left mouse button:

```
click <elem:element>
```

Examples:
```
click button "Sign in"

element logout = button "Sign out"
click logout
```

### context ... as ... end

The **context as** statement considers the whole web page or the contents of the referenced HTML element in the web page to match the referenced page object model:

```
context <elem:element> as <page>
  <statement>...
end
```

If the &lt;elem> is present, the elements referenced inside the body of the **context as** statement are searched in the context of this &lt;elem> HTML element. The &lt;elem> part itself is optional: if it is not present, we take the whole web page as the context.

If the &lt;page> is present, the statements inside the body of the **context as** statement can refer to the HTML elements defined as variables inside the &lt;page>. The search is performed in the context of the &lt;elem> HTML element. The **as** &lt;page> reference is optional.

Although both the &lt;elem> and the **as** &lt;page> are optional, at least one of them must be present.

Example of a whole web page object model:

```
page Calculator
  element display = input "number display"
  element clear = button "AC"
  element add = button "+"
  element subtract = button "-"
  element multiply = button "×"
  element divide = button "/"
  element compute = button "="
  
  operation binaryOperation(string left, element op, string right)
    click clear
    fill display with left
    click op
    fill display with right
    click compute
  end
  
  operation multiply(string left, string right)
    binaryOperation using left, multiply, right
  end
end

open "https://www.calculatorsoup.com/calculators/math/basic.php"
context as Calculator
  wait until display exists
  print "Page opened"
  multiply using "23","6"
  assert display is "138"
  capture display
end
```

Example of an HTML element which represents a confirmation dialog (the "Yes" and "No" buttons are looked up inside the `<div>` tag with a "popup" identifier):

```
page MessageBox
  element yes = button "Yes"
  element no = button "No"
end

click button "Delete"
context div "popup" as MessageBox
  click yes
end
```

The same example without a page object model:

```
click button "Delete"
context div "popup"
  click button "Yes"
end
```

### print

The **print** statement writes its parameters concatenated into a single line to the application log:

```
print <value>...
```

The arguments are separated by commas and they can be expressions of any type.

The **print** statement can help following the execution of the test, and may even help in creating user manuals.

Examples:

```
print "Page opened"

string alice = "alice"
string alicePass = "secret"
login using alice, alicePass
print "Logged in as ", alice
```

### assert ...

The **assert** statement chechs whether the given condition is true:

```
assert <condition:boolean>
```

If the condition is false, the JUnit test must result in a Failure.

Example:

```
test login
  fill input "username" with "alice"
  fill input "password" with "secret"
  click button "Sign in"
  assert label "Signed in as: Alice" exists
  assert button "Sign out" exists
end
```

### wait

The **wait** statement can be useful when the loading of a page takes a lot of time, or we want to wait for the results of some task.

The **wait ... seconds** statement waits for the given number of seconds:

```
wait <time:integer> seconds
```

The **wait until ...** statement waits until the condition becomes true:

```
wait until <condition:boolean>
```

The two can be combined if we want to wait for the condition to become true, but we want to wait with a timeout:

```
wait <time:integer> seconds until <condition:boolean>
```

Example for waiting for 10 seconds:

```
wait 10 seconds
```

In the following example, after logging in to a web page, we wait for the appearance of the "Sign out" button for at most 5 seconds:

```
integer timeout = 5
element signout = button "Sign out"
login using "alice","secret"
wait timeout seconds until signout exists
```

## Expressions

Expressions are used to define the values of variables, and also expressions are the arguments of the various statements. This section lists the possible expressions in the WebTest language.

### ... is ...

The **is** expression checks whether the contents of the given HTML element is the same as the given value:

```
<elem:element> is <value:string>
```

The type of the **is** expression is **boolean**.

The following example checks whether the user name appears on the web page after logging in:

```
login using "alice", "secret"
element loggedIn = div "loggedIn"
assert loggedIn is "Logged in as: alice"
```

### ... contains ...

The **contains** expression checks whether the contents of the given HTML element contains the the given value:

```
<elem:element> contains <value:string>
```

The type of the **contains** expression is **boolean**.

The following example checks whether the user name appears on the web page after logging in:

```
login using "alice","secret"
element loggedIn = div "loggedIn"
assert loggedIn contains "alice"
```

### ... exists

The **exists** expression checks whether the given HTML element exists:

```
<elem:element> exists
```

The type of the **exists** expression is **boolean**.

The following example checks whether the "Sign out" button appears on the web page after logging in:

```
login using "alice","secret"
element logout = button "Sign out"
assert logout exists
```

### not ...

The **not** expression returns the logical negation of the given value:

```
not <value:boolean>
```

The type of the **not** expression is **boolean**.

Example:

```
login using "alice","secret"
element logout = button "Sign out"

if not logout exists then
  print "Sign out is not displayed after signing in."
end
```

### constant expressions

Constant expressions are the following:

* character strings (of type **string**): between quotation marks with the conventional Java syntax (e.g., "Hello World!"), but we can also use ampersands instead of quotation marks (e.g., 'Hello World!')
* integer number (of type **integer**): using the conventional Java syntax (e.g., 10)
* logical value (of type **boolean**): **true** or **false**
* HTML element (of type **element**): `<tag> <label>`

In case of an HTML element, the tag (`<tag>`) and the label (`<label>`) help to identify the element unambiguously. The Selenium library provides a lot of [strategies](https://www.selenium.dev/documentation/webdriver/elements/locators/) to identify elements (e.g., based on the id attribute, the name attribute, the css class, or xpath). In the WebTest language we hide all these options by trying the &lt;tag>-&lt;label> values along with multiple strategies, and the strategy returning in the first unambiguous result is applied to select the HTML element. The potential strategies and their order of evaluation is the following:

* xpath: `.//<tag>[@id='<label>']`
* xpath: `.//<tag>[@name='<label>']`
* xpath: `.//<tag>[@aria-label='<label>']`
* xpath: `.//<tag>[@aria-labelledby='<label>']`
* xpath: `.//<tag>[@label='<label>']`
* xpath: `.//<tag>[@for='<label>']`
* css selector: `#<label> tag`
* xpath: `.//<tag>[@placeholder='<label>']`
* xpath: `.//<tag>[@value='<label>']`
* xpath: `.//<tag>[text()='<label>']`
* xpath: `.//<tag>[contains(text(),'<label>')]`
* xpath: `.//<tag>[contains(@class,'<label>')]`

If none of the strategies finds an unambiguous result, we take the element as non-existent on the page.

Examples for constant expressions:

```
string hello = "Hello World!"
if true then
  wait 10 seconds
  click button 'Hello'
end
```

