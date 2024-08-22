# Code generation

After the Xtext compiler processed and checked the WebTest codes, we have to generate JUnit tests from them automatically. In essence, we have to compile WebTest codes to Java codes.

The generated Java codes must be embedded into the project structure created by the [project wizard](TaskProjectWizard.md), and they must be able to be compiled and executed within this project.

The recommended way of writing the code generator is by using the [Xtend](https://eclipse.dev/Xtext/xtend/documentation/index.html) language and it's [templates](https://eclipse.dev/Xtext/xtend/documentation/203_xtend_expressions.html#templates). So in this task it is recommended to write files with **xtend** extension.

## Preparing the code generation

Before we start writing a code generator, it is recommended to write the codes manually, which we would like to produce automatically. These manually written codes can serve as examples for writing the templates of the generator.

Fortunately, we have already written these example codes in the **webtest.example** project for you. This project contains both WebTest codes and the Java codes which should be produced from them. It is recommended, although not necessary, to produce exactly these Java codes from your own generator. However, it is enough, if the behavior of the Java code you produce is the same as the behavior of the example code.

Take some time to examine the **webtest.example** project and its contents: the WebTest codes, the JUnit tests produced from them, and the utility classes (**Page**, **PageElement**, **SeleniumTest**) for these tests!

In the [project wizard](TaskProjectWizard.md) we have already prepared the project structure for you, and we have also built in the generation of the utility classes (**Page**, **PageElement**, **SeleniumTest**). Don't touch the project structure and the utility classes! (Except maybe the value of the **DEFAULT_CHROME_DRIVER_LOCATION** constant in the generated **SeleniumTest.java** class, if necessary.)

In this task you only have to produce a single Java file from each WebTest file. Each Java file represents a JUnit test that corresponds to the WebTest file.

## The skeleton of the generator

To help you get started, we provide the skeleton of the generator for you. Inside the **webtest.generator** project under the **src** folder in the package **webtest.generator** create a **UnitTestGenerator.xtend** file with the following content:

```Java
package webtest.generator

import webtest.model.Main
import webtest.model.Operation
import webtest.model.Page
import webtest.model.TestCase
import webtest.model.Type
import webtest.model.Variable

class UnitTestGenerator {
    def generate(Main main) {
        val testClassName = main.testClass.get(main.testClass.length-1)
        '''
        package «main.testClass.take(main.testClass.length-1).join(".")»;
        
        import org.junit.jupiter.api.Test;
        import org.openqa.selenium.SearchContext;
        import org.openqa.selenium.WebDriver;
        import org.openqa.selenium.WebElement;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;

        import webtest.selenium.api.Page;
        import webtest.selenium.api.PageElement;
        import webtest.selenium.api.SeleniumTest;

        public class «testClassName» extends SeleniumTest {
            private static Logger logger = LoggerFactory.getLogger(«testClassName».class);

            «FOR tc: main.declarations.filter(TestCase)»
            @Test
            public void «tc.name»() {
                // TODO
            }
            «ENDFOR»
            
            «IF main.body.statements.length > 0»
            @Test
            public void body() {
                // TODO
            }
            «ENDIF»
            
            «FOR op: main.declarations.filter(Operation)»
            private void «op.name»(«FOR param: op.parameters SEPARATOR ", "»«param.generateType» «param.name»«ENDFOR») {
                // TODO
            }
            «ENDFOR»
            
            «FOR page: main.declarations.filter(Page)»
            private static class «page.name» {
                private SearchContext context;
                
                public «page.name»(SearchContext context) {
                    this.context = context;
                }
                
                «FOR op: page.operations»
                public void «op.name»(«FOR param: op.parameters SEPARATOR ", "»«param.generateType» «param.name»«ENDFOR») {
                    // TODO
                }
                «ENDFOR»
                
                «FOR v: page.variables»
                public «v.generateType» get«StringUtils.toPascalCase(v.name)»() {
                    // TODO
                }
                «ENDFOR»
            }
            «ENDFOR»
        }
        '''
    }
    
    def static generateType(Variable variable) {
        val type = variable.type
        if (type == Type.ANY) return "Object"
        if (type == Type.INTEGER) return "int"
        if (type == Type.STRING) return "String"
        if (type == Type.BOOLEAN) return "boolean"
        if (type == Type.ELEMENT) return "PageElement"
        return "error";
    }
    
}
```

This generator can produce a minimal JUnit test, in which the bodies of the methods are empty. You can write your own code at the *TODO* lines, but feel free to modify other parts of the generator, if necessary. In fact, for some extensions, it will be necessary. The only goal of the example above is to demonstrate the capabilities of Xtend and to help you get started.

We also need to make Xtext call our generator. Inside the **webtest.dsl** modify the **webtest.dsl.generator.WebTestDslGenerator** class as follows:

```Java
package webtest.dsl.generator;

import java.util.stream.Collectors;

import org.eclipse.emf.ecore.resource.Resource;
import org.eclipse.xtext.generator.AbstractGenerator;
import org.eclipse.xtext.generator.IFileSystemAccess2;
import org.eclipse.xtext.generator.IGeneratorContext;

import webtest.generator.UnitTestGenerator;
import webtest.model.Main;

/**
 * Generates code from your model files on save.
 * 
 * See https://www.eclipse.org/Xtext/documentation/303_runtime_concepts.html#code-generation
 */
public class WebTestDslGenerator extends AbstractGenerator {

    @Override
    public void doGenerate(Resource resource, IFileSystemAccess2 fsa, IGeneratorContext context) {
        if (resource == null) return;
        var contents = resource.getContents();
        if (contents == null || contents.size() == 0) return;
        if (contents.get(0) instanceof Main) {
            var main = (Main)contents.get(0);
            var unitTestGenerator = new UnitTestGenerator();
            var path = "./test/java/"+main.getTestClass().stream().collect(Collectors.joining("/"))+".java";
            var code = unitTestGenerator.generate(main);
            fsa.generateFile(path, code);
        }
    }
}
```

This code retrieves the **Main** root of our WebTest model from the **resource** parameter, passes it to your generator, and saves the generated Java code into the **src-gen/test/java** folder.

## Writing the generator

Modify the **UnitTestGenerator** class we have created in the previous section so that it produces a working JUnit test that follows the behavior defined in the WebTest code!

Don't forget to incorporate the two extensions into your generator!

***HINT:** The [dispatch](https://eclipse.dev/Xtext/xtend/documentation/202_xtend_classes_members.html#polymorphic-dispatch) method declaration of Xtend can be useful when you are generating statements and expressions. Dispatch methods are called at runtime based on the dynamic type of the arguments, so dispatch methods can be useful to avoid the heavy use of **instanceof**.*

***HINT:** If you start the **Runtime Eclipse** in Debug mode, and you make only minor changes in the generator, the changes will be automatically applied, and you don't have to restart the **Runtime Eclipse** all the time. You only need to make a slight modification in the WebTest code and save it to run the generator again. Eclipse will tell you if the modification in the generator was major, and you need to restart the **Runtime Eclipse**.*

## Check the solution

You can check the correct behavior of the generator with the following steps:

1. Start the **Runtime Eclipse**
2. Create a new **WebTest project** using the wizard
3. Add a new WebTest file with a **.wt** extension to the **webtest** folder
4. Write some tests inside the newly created file using the WebTest syntax, while enjoying the IDE support for the language: syntax highlighting, outline, validation, etc.
5. Save the WebTest file. The corresponding JUnit test must be generated automatically.
6. Right click on the project, and select **Run As > JUnit Test**. The JUnit test should execute successfully while controlling a browser through the Selenium library and performing the steps written in the WebTest file.

## To be uploaded

During the solution of the task, take screenshots taken from the following parts, and upload them into the folder **homeworks/hw2** of your own git repo:

* The project structure of a project created by the project wizard in **Runtime Eclipse**. Open the folders so that all the generated files are visible.
* An opened **.wt** file of at least 20 lines in the project's **webtest** folder, which contains examples for your two extensions.
* The generated Java code from this **.wt** opened in **Runtime Eclipse**.
