# WebTest language extensions

## Capture extension

The **capture page** keyword takes a screenshot of the web page:

```
capture page
```

The **capture** keyword can accept an HTML element as a parameter:

```
capture <elem:element>
```

In the latter case the **capture** keyword scrolls the page such that the specified element becomes visible, it highlights the element with a red frame, takes a screenshot, and finally, removes the highlight.

For scrolling and highlighting the following JavaScript code can be used, where `arguments[0]` is the element to be shown and highlighted:

```javascript
arguments[0].style.outline = 'red solid 4px'; arguments[0].style.outlineOffset = '-4px'; arguments[0].scrollIntoView({ block: 'center', inline: 'center' });
```

After the execution of the code above, it is advisable to wait for 1 second to make sure that the command is ececuted, and only take the screenshot after this.

To remove the highlight, the following JavaScript code can be used:

```javascript
arguments[0].style.outline = ''; arguments[0].style.outlineOffset = '';
```

The **capture** statement saves the screen shot as a PNG image into the `output` folder, and prints `Captured: <filename.png>` to the application log. The name of the file must be unique (e.g., linear numbering).

The **capture** statement may help in creating user manuals.

Example:

```
print "Click Sign in to log in:"
capture button "Sign in"
click button "Sign in"
```

## ForEach extension

If an HTML is not uniqe according to the `<tag> <label>` combination, it may be necessary to iterate through all of the possibilities (e.g., the elements of a list or the rows of a table). This is what the **foreach** loop is for:

```
foreach <item:element> in <list:element>
  <statement>...
end
```

Let's take the following HTML code:

```html
<ul id="Greetings">
  <li>Hello, Alice</li>
  <li>Hello, Bob</li>
</ul>

<ol id="Beverages">
  <li>Coffee</li>
  <li>Tea</li>
  <li>Milk</li>
</ol> 

<table id="Companies">
  <tr>
    <th>Company</th>
    <th>Contact</th>
    <th>Operations</th>
  </tr>
  <tr>
    <td>Company A</td>
    <td>CEO A</td>
    <td><input type="button" value="Edit"/><input type="button" value="Delete"/></td>
  </tr>
  <tr>
    <td>Company B</td>
    <td>CEO B</td>
    <td><input type="button" value="Edit"/><input type="button" value="Delete"/></td>
  </tr>
</table> 
```

Examples of using **foreach** for the HTML code above:

```
context ul "Greetings"
  element greetings = li ""
  foreach message in greetings
    assert message contains "Hello"
  end
end

context ol "Beverages"
  foreach beverage in li ""
    print beverage
  end
end

context table "Companies"
  foreach company in tr ""
    click button "Edit"
  end
end
```

A more complex example where each row of a table is represented by a page object model:

```
page CompanyOperations
  element edit = button "Edit"
  element delete = button "Delete"
end

context table "Companies"
  foreach company in tr ""
    context company as CompanyOperations
      click delete
    end
  end
end
```

## JavaScript extension

The **javascript** keyword executes the given JavaScipt code:

```
javascript <code:string>
```

We can specify arguments separated by commas for the **javascript** keyword after the **using** keyword:

```
javascript <code:string> using <value>...
```

An example that displays an alert dialog box:

```
javascript 'alert("Hello World!");'
```

An example that sets the value of the search field to the given search text:

```
element searchField = input "q"
javascript 'arguments[0].value=arguments[1];' using searchField, "jwst"
```

An example that scrolls the page so that the search button becomes visible:

```
javascript 'arguments[0].scrollIntoView(true);' using button "Search"
```

## Manual extension

The structure of a **manual** is the following:

```
manual <name>
  <statement>...
end
```

A manual must have a name (&lt;name>). The body of the manual is a list of statements. Manuals are independent of each other and can be executed in any order. The output of a manual (results of the **print** and **capture** statements) must be written into an HTML file in the `output` folder, which serves as a user manual for the given web page. The name of the file must be the same as the name of the manual.

Example for a manual:

```
manual LoginLogout
  print "<h1>Login-logout</h1>"
  print "<p>Type the user name to log into the application:</p>"
  fill input "username" with "alice"
  capture input "username"
  print "<p>Then type the password:</p>"
  fill input "password" with "secret"
  capture input "password"
  print "<p>Finally, click the <b>Sign in</b> button:</p>"
  capture button "Sing in"
  click button "Sing in"
  print "<p>Click the <b>Sign out</b> button to log out:</p>"
  capture button "Sing out"
  click button "Sing out"
end
```

After executing the example above, the following **LoginLogout.html** must be produced:

```html
<html>
    <body>
        <h1>Login-logout</h1>
        <p>Type the user name to log into the application:</p>
        <img src="screenshot001.png" /> 
        <p>Then type the password:</p>
        <img src="screenshot002.png" /> 
        <p>Finally, click the <b>Sign in</b> button:</p>
        <img src="screenshot003.png" /> 
        <p>Click the <b>Sign out</b> button to log out:</p>
        <img src="screenshot004.png" /> 
    </body>
</html>
```

The resulting HTML can be made nicer using CSS styles (e.g., bootstrap).

The structure of a WebTest file with the addition of manuals becomes:

```
webtest <package>.<class>

(<page> | <test> | <manual> | <operation>) ...

<statement>...
```

At the beginning of the file the **webtest** keyword defines the Java class (&lt;class>) with full package prefix (&lt;package>) that will result in a JUnit test by compiling the WebTest file. After this, any number of web page model (&lt;page>), test case (&lt;test>), manual (&lt;manual>) and operation (&lt;operation>) may follow in any order. Finally any number of statements (&lt;statement>) may come.


## TestParams extension

A parameterized test case has the following structure:

```
test <name>(<parameter>...)
  with <value>...
  <statement>...
end
```

After the name of the test, a list of comma separated parameters can be specified enclosed within parenthesis. The parameter list is optional. If it is not present, the parenthesis must be omitted. If it is present, after the **with** keyword we can specify the arguments separated by commas. The **with** keyword can be used multiple times: each use corresponds to a different instance of the test case. These parameterized tests must be mapped to [JUnit 5 parameterized tests](https://www.baeldung.com/parameterized-tests-junit-5).

Example:
```
test login(string username, string password)
with "Alice","secretA"
with "Bob","secretB"
  fill input "username" with username
  fill input "password" with password
  click button "Sign in"
  assert label "Signed in as:" contains username
  assert button "Sign out" exists
end
```
