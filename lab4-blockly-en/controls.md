
# Blockly4HTML Language Specification

Firstly, we a short overview is presented, then the language elements are listed below and briefly elaborated.
The list does not include extensions(extensions.md), but only common language elements. 

## Overview
The purpose of the language is to produce working websites without the programming skills or HTML experience.

### Basic blocks
* **WebPage**
  * The language always contains the exact definition of a web page (WebPage) (Webpage itself is included in the published [example](init_example.md))
  * Display in the workspace by default (should not need to add manually)
  * Attributes:
    * Title (text): the title element of the HTML web page
  * Child elements:
    * Elements: contained HTML elements (page content)
* **Table**
  * Element for specifying a table
  * Child elements:
    * Rows: table rows 
* **Row**
  * Specifies one row in the table
  * Child elements:
	  * Height: specifies the height of the row (value-based relationship)
      * Cell: cells in the row
* **Cell**
  * Specifies one cell in the table
  * Child elements:
	  * Width: specifies the width of the cell (value-based relationship)
	  * Children: HTML elements that form the contents of the cell  	
* **Text**
    * A block of text (&lt;p&gt;) element to be used.
    * Attributes:
      * Text (text): the text to be displayed  
 * **HeaderText**
   * A title text (e.g. &lt;h1&gt;) element to be used.
   * Attributes:
      * Type: the style h1/h2/h3/h4 can be set
	  * Text (text): the text to be displayed  
  * **Image**
    * An element for inserting an image
    * Attributes:
      * Href (text): the URL of the web page where the image can be found
	* Child elements:
		* Width: image width (value based relationship!)
		* Height: image height (value based relationship!)
  * **Div**
    * HTML block (&lt;div&gt;) specifying element
     * Child elements:
		* Width: block width (value based relationship!)
		* Height: block height (value based relationship!)
        * Padding: the inner margin of the block (uniform in all directions) (value-based relationship!)
        * Children: HTML elements that form the contents of the block and are subject to the specified formatting        
  * **Size**
     * Specifies a length or width value (e.g. 25px)
	 * Attributes:
	    * Amount (text): the specific value (e.g. 25)
	    * Type: px (pixels), or % (percent)

### Input

* **Form**
  * Container form for the input fields
  * Attributes:
    * Action (text): the task to perform when submitting the form (the destination URL)
  * Child elements:
    * Children: contained HTML elements
* **Input**
   * Input element
   * Attributes:
     * Type: field type: Password/Text/Checkbox/Number
     * ID (text): the ID of the field
     * Caption: the field caption
   * Child elements:
     * Validator: validator linked to the field (value-based relationship)
 * **Button**
   * Form submission item
   * Attributes:
     * Label (text): the button label
        
### Validation
* **Required**
    * Specifies whether linked element is required
* **MaxLength**
    * Specifies the maximum length of the linked element
    * Attributes:
      * Value: maximum length (20..255)
* **Pattern**
    * Specifies the pattern expected from the linked element
    * Attributes:
      * Value: the expected pattern as a regular expression
* **ValidatorMsg**
    * Allows you to specify your own validation error message
    * Attributes:
      * Message: the validation error message to display
   * Child elements:
     * Validator: a validator to customize (value-based relationship)

        
### Restrictions
   
   The following constraints must be specified in the editor:
   * Only Size items can be bound into Width/Height/Padding fields
   * Directly within Table elements we accept Row elements only, while within Rows, we accept Cells only
   * Only elements of type Validator (Required, MinLength, Pattern) can be bound to the Validator field of the ValidatorMsg element
   * Only a Validator element or a ValidatorMsg can be entered into the Validator field of the Input element

Here are some [tips](hints.md) to help you solve tasks if you get stuck.
