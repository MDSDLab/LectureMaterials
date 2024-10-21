# WebTestInputValidation Language Specification

Below, after introducing the purpose of the language, its elements are listed and briefly presented. The list does not include [extensions](Extensions.md), only the common language elements that all teams must implement. Examples of the language elements described here can be found in the input [example code](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/PersonForm.wtiv).

## Overview
The purpose of the language is to provide input files in the form of HTML and JavaScript files for the WebTest language, which exclusively describe the validation of HTML **forms**. The language defines HTML **input** tags, specifying the validation of the elements in addition to the structure. From the structure definition, we can generate HTML, and from the validation code, we can generate JavaScript. During validation, we can use built-in HTML validation, validate based on the values of other **input** elements, or control their visibility.

## Form Definition
The language always contains exactly one form definition, specifying its ID.

```
form <id>

...

end
```

## Input Definition
Within the form, any number of input fields can be defined.

### Structure
When defining an input field, we can optionally specify whether it should be hidden (**hidden**) when the page loads. Additionally, we must specify the input [type](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input), its ID, and name. Currently, the language only supports the following types: **text**, **number**, **date**, **checkbox**. After this comes the validation block.

```
<hidden> input <type> <id> <name>
{
    <validation>
}
```

### HTML Attribute Validation
The language supports built-in HTML input validation [attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#attributes). Among these, we only support the following: **minlength**, **maxlength**, **min**, **max**, **step**, **pattern**, **required**. Except for **required**, all must also have a parameter. Since the goal of the language is to provide input for automatically generated test cases, the built-in HTML validation feedback must be disabled during code generation, and custom error messages should be provided instead. This can be done using the "=>" syntax.

```
<attribute validation> <parameter> => <error message>
```

---
**Note**

Since setting the **maxlength** attribute in the browser automatically limits the size of the text box, we cannot display custom error messages using JavaScript, as the error (overflow) cannot be triggered. Nevertheless, for completeness, **maxlength** is included in the language alongside **minlength**.

---

### Comparative Validation
The language also allows for simple comparisons with the values of other input fields: **eq** (equal), **neq** (not equal), **lt** (less than), **gt** (greater than). Each of these has exactly one parameter: the ID of another input field.

```
<comparative validation>(<input id>)
```

### Visibility Control
In addition to specifying validation rules, for **checkbox** type input fields, we can also control the visibility of another input field or group using **show** or **hide**. This is done using the **on?** and **off?** syntax, which refers to the checked or unchecked state change of the checkbox. We must also support inverse operation, meaning that if a checkbox shows another field when checked, the field must disappear when unchecked. A **checkbox** input field can have any number of visibility rules.

```
<on | off>? <show | hide> <id>
```

## Group Definition
In addition to input fields, a form can also contain groups. A group can contain any number of inputs, but cannot contain other groups. A group also has an ID. All IDs must be globally unique across input fields and groups. The **hidden** state can also optionally be specified for a group.

```
<hidden> group <id>
{
    <inputs>
}
```