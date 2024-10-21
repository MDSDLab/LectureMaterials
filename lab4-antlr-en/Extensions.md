# Extensions Description

This task involves three extensions, with each [team](ExtensionsTable.md) being assigned one. The other two extensions can be optionally completed but will not result in extra points. In the table, "Error Message" refers to the first extension, "JavaScript" to the second, and "Concatenation" to the third.

## Custom Error Message Enhancement

Make custom error messages more flexible: allow references to the relevant input field properties (id, type, name) in the message using **%id%**, **%name%**, **%type%**, which will appear in the generated JavaScript code. Additionally, allow referencing the field's value using **%value%**, which requires special handling during code generation (use ```.val()``` for jQuery selectors).

Examples (additionally, generated code examples can be found [here](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/validation.js)):
```
required => "Input %id% '%name%' of type %type% is mandatory!"
```

```
pattern("[A-Z][a-z]*") => "Last name (%value%) must begin with a capital letter!"
```

## Native JavaScript Validation Support

Introduce support for native JavaScript validation. Allow executing native JavaScript code within a JS block, where each statement is followed by its own custom error message, separated by a semicolon. The statements do not need to be analyzed and can be treated as raw text (HINT: use a very general description in the grammar). During code generation, each statement is executed on the input field's value, and the error message is displayed if the code returns false.

Example (additionally, generated code examples can be found [here](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/validation.js)):
```
JS
{
    startsWith("A"); => "First name must start with 'A'!"
    substring(1, 4) == "abc"; => "First name must contain 'abc' after the first character!"
}
```

---
**Note**

You do not need to address security concerns or perform any validation on the JS block.

---

## String Concatenation Introduction

Extend Comparative Validation: in addition to simple id parameters, allow the concatenation of id and string literal expressions. In the generated code, concatenation should always result in a string. This can be easily handled due to JavaScript's inherent behavior.

Example (additionally, generated code examples can be found [here](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/validation.js)):
```
eq(lname + "ov") => "First name must be last name + ov!"
```