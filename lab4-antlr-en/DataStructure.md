# Building the Underlying Data Structure

After creating the [grammar](Grammar.md), the next step is to build a data structure that can later be used during semantic analysis and code generation. This is similar to constructing a classic symbol table, but in our case, it is simpler. It is recommended to use the **main.WebtestInputTableBuilder** class (applying the visitor pattern) for building the structure, and the **data.WebtestInputTable** class for maintaining the table. As in classical symbol table construction, it is advisable to handle error management at this stage, as it will make semantic analysis easier.

For storing input field data, the **data.WebtestInput** class can be used, but additional helper classes can also be created. It is also worth considering how to store the structure of groups.

Since the subsequent tasks heavily rely on this step, we outline the solution steps here:
- Extend the **WebtestInput** class with the appropriate attribute
    - Consider what information we want to store about the *input* elements and how to handle *group* elements
    - **IMPORTANT**: No auxiliary structure or storage is needed for validations within *input* elements; this significantly simplifies the task!
- Extend the **WebtestInputTable** class so it can store **WebtestInput** objects
    - You will definitely need *lookup* and *insert* operations, but you can add other operations as well
- Extend the **WebtestInputTableBuilder** class with the appropriate *visit* methods, which will build the table (the table is stored in the **WebtestInputTable** attribute)