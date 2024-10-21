# Semantic Analysis

Using the [underlying data structure](DataStructure.md), the full semantic analysis can now be performed. With the help of the **main.validation.WebtestInputSemanticAnalyzer** - applying the visitor pattern - the following tasks must be addressed, with each case requiring a dedicated semantic error (**main.validation.WebtestInputValidationErrorHandler**). Although some of these checks could be enforced by modifying the grammar, we explicitly require custom error messages for the following cases, so **adjusting the grammar rules is not an acceptable solution** to handle them. Custom error messages must be provided!

- *minlength*, *maxlength*, and *pattern* type HTML attribute validations are only allowed in *text* type input fields
- *min* and *max* type HTML attribute validations are only allowed in *number* and *date* type input fields
- *step* type HTML attribute validations are only allowed in *number* type input fields
- Comparative validations (*eq*, *neq*, *lt*, *gt*) are not valid for *checkbox* type input fields
- In comparative validations, the parameter type must match the type of the associated *input* field
- In all cases where another input field is referenced by its id, the referenced input field must exist
- Visibility control (*show*, *hide*) is only allowed for *checkbox* type input fields
- Visibility control must not contain contradictory logic (e.g. "on? show fname" and "off? show fname")