# Extensions

This task involves the following extension:

## Validator 

 According to the basic task, everything can be used for any input, which, however, would not always result in valid choices. Let's complete the editor to indicate invalid combinations with a warning:

 * In the case of CheckBox and Number the validation MaxLength or Pattern cannot be used
 * Introduce a new validator, which can only be used for Password-type elements and which can be used to specify their minimum length!

Certain rules must also be checked for input fields:

 * In the text field of the Text and HeaderText elements, only alphanumeric characters are allowed, the rest should be remove automatically from the field at the end of editing (e.g. &lt;br&gt; --> br)!
