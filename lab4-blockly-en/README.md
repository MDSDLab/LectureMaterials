# Lab 4 (Part 2) – HTML Website Editor Based on Blockly

## Steps to complete the lab task

1. Read the Blockly [Tutorial](materials/blockly_tutorial.pdf) task. The tasks in this description are not related to the lab task, but they help you get started in the world of Blockly. Tasks can be completed in the released version of Blockly.
2. From the [mdsd-2024-lab4-blockly-en](https://github.com/MDSDLab/mdsd-2024-lab4-blockly) repo, copy the **Blockly** folder directly to the root of your repo.
3. Complete the tasks described below, taking into account the [add-ons] assigned to you(extensions.md).
4. Create a **tag** named **hw4-blockly** for the last commit.

## HTML editing language, Blockly-based editor and code generator

The goal of this lab task is to create a language (Blockly4HTML) in the Blockly environment, which can be used to create web pages that contain basic HTML elements. The logic for the website described in Blockly must be generated by customizing the initial code generator that has been released. 

*The task is only loosely related to the WebTest language, unlike the ANTLR task, it is not intended to produce a web page that can be used directly by WebTest at the end of generation.*

Both the [common part](controls.md) of the Blockly4HTML language and the [extension](extensions.md) must be implemented.

## Task description

To complete the lab task, open the published starter project (by opening the *index.html* page) and read the [description](init_example.md). This is a minimal example, where basic functions can be tested, but the HTML elements to be implemented are still missing. The Blockly language contains a single element, but you can try out code generation and generate the final state (working website snippet) and download it for ease of use. 

The lab task is considered complete when all the listed HTML elements ([base elements](controls.md) and the [extension](extensions.md)) is modeled (included in the language and used in the editor) and generated correct (working, semantically appropriate) HTML code from them. Due to the nature of Blockly, automated test cases are not provided for this task, but you can manually verify that the solution meets the requirements. 

*The task is acceptable only if it applies at least minimally the basic ergonomic principles, i.e. each block type has a name (which can be seen in the editor) or different colors are used by each type.*

### Scoring

Lab 4 consists of two separate tasks; the maximum number of points can be earned for **[ANTLR](https://github.com/MDSDLab/LectureMaterials/blob/main/lab4-antlr-en/README.md) task is 15 points** and for **Blockly task is 10 points**. The scoring of the Blockly task is distributed as follows:

| Common parts | Ergonomics | Code Generation | Add-on |
| :-: | :-: | :-: | :-: |
| 2 | 1 | 4 | 3  |

## References

Useful links:

* Some [tips](hints.md) to solve tasks
* Blockly [Block factory](https://blockly-demo.appspot.com/static/demos/blockfactory/index.html)
* Blockly [documentation](https://developers.google.com/blockly/guides/overview)
* HTML markup language in Blockly (partly similar to lab task) [link](http://blocklyhtml.zgtm.de/)
* Template literal (for code generation) [link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
* Blockly field demo [link](https://google.github.io/blockly-samples/)
* Input validation [link](http://bekawestberg.me/blog/types-1/)
* Custom validation [link](https://blocklycodelabs.dev/codelabs/validation-and-warnings/index.html#0)
