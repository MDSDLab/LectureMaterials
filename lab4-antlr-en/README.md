# Laboratory 4 (Part 1) - ANTLR Compiler and Code Generator

## Steps to solve the lab

1. Copy the contents of the [mdsd-2024-lab4-antlr](https://github.com/MDSDLab/mdsd-2024-lab4-antlr) repo into your own repository, into a newly created **webtest-input-validation** folder!
2. Solve the tasks described below, taking into account the extension assigned to you!
3. Create a **hw4-antlr** **tag** on the last commit!

## Creating a language for HTML form validation (WebTestInputValidation)

The goal of this lab task is to develop a new WebTestInputValidation language, separate but supportive of the WebTest language, using ANTLR, and to create the corresponding code generators. The task of the compiler is to build an internal data structure from the input (.wtiv extension) files, and to find and report semantic errors to the user (semantic analysis). Afterward, it should generate exactly one HTML and one JavaScript output file from the input file, which can be practically used to run the tests previously described in the WebTest language. Running the tests is not part of the task (this would only be a practical application of the developed language), so if you did not manage to implement the code generator during the 2nd lab, it will not cause any disadvantage in the 4th lab, nor any point deduction.

The common part of the WebTestInputValidation language must be implemented by everyone. In the provided input example code, there is an example for each extension (marked with a comment), but each team only needs to support one extension; supporting the other extensions is optional.

## Modifying the initial project
The initial project provides a skeleton for the solution. The skeleton is just a recommendation and can be modified, classes can be added or removed, etc., with one constraint: running the main method of the **WebtestInputRunner** class must execute both the semantic analysis and the code generation, as it is right now in the initial project!

## List of the tasks

To complete the lab task, open the initial project with [IntelliJ IDEA](https://www.jetbrains.com/idea/).

Preparations:

0. [Preparing the project](Technical.md)

Creating the [WebtestInputValidation](WebTestInputValidationSpecification.md) language:

1. [Creating the grammar](Grammar.md)
2. [Building the underlying data structure](DataStructure.md)
3. [Semantic analysis](SemanticAnalysis.md)

Code generation:

4. [HTML code generation](HTMLGeneration.md)
5. [JavaScript code generation](JSGeneration.md)

Extensions:

6. [Description of extensions](Extensions.md)
7. [Assignment of extensions](ExtensionsTable.md)

## Evaluation

In the initial project (**examples** package), there is an input [example code](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/PersonForm.wtiv), which contains all language elements of the WebTestInputValidation language. Additionally, the generated [HTML](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/PersonForm.html) and [JavaScript](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/validation.js) files from the example code can also be found here (**examples.generated** package). The grammar and code generation can be considered complete if the input file can be fully read and interpreted, and if functionally equivalent code to the above can be generated from it. Semantic analysis can be considered complete if an appropriate error is generated for every case described in the task description.

The solution can also be verified with automatically generated Selenium tests similar to the [example](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/FormTest.wt) written in the WebTest language, which can also be found in the **examples** package.

### Scoring

Laboratory 4 consists of two separate tasks; a maximum of **15 points** can be earned for the **ANTLR task** and **10 points** for the **[Blockly](https://github.com/MDSDLab/LectureMaterials/blob/main/lab4-blockly-en/README.md) task**. The points for the ANTLR task are distributed as follows:

| Grammar | Semantic analysis | Code generation |
| :-: | :-: | :-: |
| 3 points | 3 points | 9 points |



## References

Useful links for completing the task:

* Materials from last semester's ANTLR practices - lexical / syntactic analysis [practice](https://github.com/bmeaut/ModellalapuSzoftverfejlesztes/tree/master/practice/practice_02) + semantic analysis / code generation [practice](https://github.com/bmeaut/ModellalapuSzoftverfejlesztes/tree/master/practice/practice_04)
    * Also includes a guide for setting up IntelliJ and ANTLR
* [ANTLR](https://www.antlr.org/) ([Documentation](https://github.com/antlr/antlr4/blob/master/doc/index.md))
* [ANTLR example grammars](https://github.com/antlr/grammars-v4)
* [StringTemplate](https://www.stringtemplate.org/) ([Documentation](https://github.com/antlr/stringtemplate4/blob/master/doc/index.md))