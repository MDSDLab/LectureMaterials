# HTML Code Generation

After the [semantic analysis](SemanticAnalysis.md), we can be confident that the code is valid. Therefore, the code generation visitor (**main.codegen.WebtestHtmlCodeGenerator**) only needs to focus on the logic of generating the HTML code.

When generating HTML code, the following points should be considered:
- The basic structure of the page (*html*, *body*, *script* tags) can be generated statically, including the imported jQuery version and the name of the generated JavaScript file (```validation.js```).
- Always generate an appropriate label for each input element.
- The *div* tags containing custom error messages should be initially hidden.
- HTML attribute validations should appear as *attributes* on the corresponding *input* fields.
- A group can be generated as a simple *div* tag.

---
**Note**

Any technology can be used for code generation; the starter project is based on [StringTemplate](https://www.stringtemplate.org/), but you are free to use any code generation framework or even a native StringBuilder. However, using StringBuilder will likely make code generation significantly more complex.

---

Here are some tips for implementing the code generator (particularly when using StringTemplate):
- Review the **WebtestInputCodeGenerator** class
    - The generated code is stored in the *generatedCode* variable, which is used by **WebtestInputRunner**. You do not need to modify this.
    - It also contains an example of simple StringTemplate usage.
- Start by generating the static skeleton of the page by extending the **html** template in the **HtmlGen** file.
    - Later, this template will need to be extended with parameters.
- It's advisable to define multiple templates in the **HtmlGen** file.
    - For example, the header of an input-generating template might look like this: ```input(type, id, name, attributes, isHidden)```.
- Since the tag opening character `<` is also used in StringTemplate, HTML tag generation requires escaping: `\<`.
- In the **WebtestHtmlCodeGenerator** class, you will need to override *visit* functions similarly to previous tasks.
    - Use the templates you created, following the patterns seen in the **WebtestInputCodeGenerator** class (for more details, refer to the StringTemplate documentation).
- During code generation, it's useful to follow the provided example: [sample code](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/PersonForm.wtiv) --> [generated HTML](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/PersonForm.html).