# JavaScript Code Generation

When generating JavaScript code, the following points should be considered:
- The use of [jQuery](https://jquery.com/) is strongly recommended.
- Upon clicking the *submit* button (not in the *submit* event handler), it is advisable to reset (hide) all error messages to their default state.
- HTML attribute validations will run before custom validations (either comparison-based or plugin-based).
    - The former should be placed in the *input* fields' *invalid* event handlers, while the latter should go in the *submit* event handler.
- Use [ValidityState](https://developer.mozilla.org/en-US/docs/Web/API/ValidityState) for handling HTML attribute validation.
- Don’t forget to generate code for the inverse logic in visibility controls.
- For comparison validations, refer to the value of the related *input* field (using jQuery selector ```.val()```) in the generated code.

---
**Note**

Any technology can be used for code generation; the starter project is based on [StringTemplate](https://www.stringtemplate.org/), but you are free to use any code generation framework or even a native StringBuilder. However, using StringBuilder will likely make code generation significantly more complex.

---

Here are some tips for creating the code generator (especially when using StringTemplate):
- Review the **WebtestInputCodeGenerator** class
    - The generated code is stored in the *generatedCode* variable, which is used by **WebtestInputRunner**. You do not need to modify this.
    - It also contains an example of simple StringTemplate usage.
- Start by generating the main event handlers (e.g. submit, submit click) by extending the **js** template in the **JsGen** file.
    - Later, this template will need to be extended with parameters.
- It’s advisable to define multiple templates in the **JsGen** file.
    - For example, the header of an HTML attribute validation template might look like this: ```htmlAttributeValidityCheck(id, errorDisplay, validityAttribute)```.
- In the **WebtestJsCodeGenerator** class, you will need to override *visit* functions similarly to previous tasks.
    - Use the templates you created, following the patterns seen in the **WebtestInputCodeGenerator** class (for more details, refer to the StringTemplate documentation).
- During code generation, it's useful to follow the provided example: [sample code](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/PersonForm.wtiv) --> [generated JavaScript](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/validation.js).