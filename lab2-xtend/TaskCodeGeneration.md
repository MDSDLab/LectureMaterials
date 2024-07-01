# Kódgenerálás (code generation)

A kódgenerálás során a feldolgozott és leellenőrzött WebTest kódokból JUnit teszteket kell automatizáltan előállítani. Lényegében a WebTest kódokat Java kódra kell lefordítani.

Az nincs előírva, hogy a generált Java kódnak hogyan kell kinéznie. Többféle alternatíva is lehetséges:

* Lehet WebTest fájlonként teljesen önállóan működő Java kódokat készíteni, ahol az egyes Java kódok között semmilyen kapcsolat sincs.
* A [projekt varázslóban](TaskProjectWizard.md) előállíthattok olyan segédosztályokat, amelyekre a WebTest fájlokból generált Java kódok építhetnek, így a generált kódok jóval egyszerűbbé válnak.

A generált Java kódoknak nem kell pontosan megegyeznie a **webtest.example** projektben szereplő mintakódokkal. (Sőt, nem is lehet pontosan ugyanazokat a kódokat előállítani, mert például a HTML elemek azonosítása be van égetve a mintakódokba, a generált kódnak azonban [dinamikusan kell kiderítenie](../lab1-xtext/WebTestReference.md#konstans-kifejez%C3%A9sek), melyik minta illesztése vezet a HTML elem megtalálásához.)

A kódgenerálás többféleképpen is megoldható, de legcélravezetőbb az [Xtend](https://eclipse.dev/Xtext/xtend/documentation/index.html) nyelv és az abban található [sablonok](https://eclipse.dev/Xtext/xtend/documentation/203_xtend_expressions.html#templates) használata.

## A generálás előkészítése

Mielőtt nekilátntok a generálásnak, célszerű manuálisan megírni azt a kódot, amit szerenétek automatizáltan előállítani. Ha egyből a generátort írjátok, akkor nagyon nagy az overhead-je a kipróbálásnak: *generátor módosítása -> Runtime Eclipse indítása -> WebTest fájl szerkesztése -> generált Java kód futtatása JUnit tesztként, feltéve, hogy egyáltalán lefordul*. Javasolt tehát először kézzel megírni egy teljes projektet, amely minden olyan elemet tartalmaz, amit generálni szeretnétek, majd az ebben lévő elemeket érdemes általánosítani és beépíteni egy generátorba.

Célszerű segédosztályokat is készíteni, amelyekre a generált kódok építhetnek. Ezekben a segédosztályokban elrejthetitek a Selenium használatának egyéb kényelmetlenségeit, például:

* A HTML elemek [dinamikus azonosítását](../lab1-xtext/WebTestReference.md#konstans-kifejez%C3%A9sek).
* A **click()** függvény nem mindig működik egy **WebElement** objektumon. Ilyenkor célszerű JavaScript kódból elvégezni a kattintást, lásd: **GoogleTest** példa a **webtest.example** projektben.
* Annak eldöntése, hogy egy HTML elem létezik-e (egyértelműen azonosítható-e) és látható-e (**isDisplayed**), lásd: **WizardTest** példa a **webtest.example** projektben.
* Egy `input` mező tartalmának törlése a szöveg begépelése előtt.
* Egy `input` mező tartalma a `value` attribútumában van, de más HTML elemeknél a **getText()** hívással kérhető el a belső tartalom.

Ezeket a segédosztályokat a [projekt varázslóban](TaskProjectWizard.md) célszerű generálni.

Ebben a részfeladatban csak a segédosztályokra építő JUnit teszteket kell generálni.

## A generátor váza

Segítségképpen megadjuk a generátor vázát, amiből könnyebben ki lehet indulni a feladat megoldása során. Hozzatok létre a **webtest.generator** projekten belül az **src** könyvtár alatt a **webtest.generator** csomagban egy **UnitTestGenerator.xtend** fájlt az alábbi tartalommal:

```
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
        
        public class «testClassName» {
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
        if (type == Type.ELEMENT) return "WebElement"
        return "error";
    }
    
}
```

Ez egy minimális JUnit tesztvázat tud generálni, amelyben az egyes függvények törzse még nincs kitöltve. A *TODO* helyekre írhatjátok a saját kódotokat, de nyugodtan hozzányúlhattok a fenti kód többi részéhez is, ha úgy látjátok célszerűnek. Sőt, egyes bővítmények esetén hozzá is kell nyúlni. A fenti példa célja mindössze az, hogy demonstrálja az Xtend nyelv képességeit, és ötleteket adjon az elinduláshoz.

Ahhoz, hogy az Xtext figyelembe vegye a generátorotokat, meg kell hívni a generátort a **webtest.dsl** projekten belül található **webtest.dsl.generator.WebTestDslGenerator** osztályból. Módosítsátok az osztályt az alábbi módon:

```
/*
 * generated by Xtext 2.30.0
 */
package webtest.dsl.generator

import org.eclipse.emf.ecore.resource.Resource
import org.eclipse.xtext.generator.AbstractGenerator
import org.eclipse.xtext.generator.IFileSystemAccess2
import org.eclipse.xtext.generator.IGeneratorContext
import webtest.generator.UnitTestGenerator
import webtest.model.Main

/**
 * Generates code from your model files on save.
 * 
 * See https://www.eclipse.org/Xtext/documentation/303_runtime_concepts.html#code-generation
 */
class WebTestDslGenerator extends AbstractGenerator {

    override void doGenerate(Resource resource, IFileSystemAccess2 fsa, IGeneratorContext context) {
        if (resource?.contents !== null &&
            resource.contents.size > 0 && 
            resource.contents.get(0) instanceof Main) {
            val main = resource.contents.get(0) as Main
            val unitTestGenerator = new UnitTestGenerator()
            val path = "./test/java/"+main.testClass.join("/")+".java"
            val code = unitTestGenerator.generate(main)
            fsa.generateFile(path, code)
        }
    }
}
```

Ez a generátor a **resource** paraméterből előbányássza a WebTest modellünk **Main** gyökerét, átadja a generátorotoknak, majd az eredményként előálló Java kódot az **src-gen/test/java** könyvtárban helyezi el.

## Feladat

Módosítsátok a **UnitTestGenerator** osztályt, hogy teljesen működő JUnit teszteket állítson elő. Szükség esetén definiálhattok segédosztályokat, amelyekre ezek a generált JUnit tesztek építenek. Ezeket a segédosztályokat ne itt, hanem a [projekt varázslóban](TaskProjectWizard.md) adjátok hozzá a projekthez.

A megvalósítandó 2 bővítmény támogatását se felejtsétek el beépíteni a generátorba!

***TIPP:** Az Xtend nyelv [dispatch](https://eclipse.dev/Xtext/xtend/documentation/202_xtend_classes_members.html#polymorphic-dispatch) függvénydeklarációja hasznos lehet az utasítások és kifejezések generálása során, amelyet meghívva futásidőben a dinamikus típus dől el, hogy melyik overload változat hívódik meg, így nem kell **instanceof**-ot használni.*

## Ellenőrzés

Az alábbi módon lehet ellenőrizni a generátor helyes működését:

1. Indítsunk el a **Runtime Eclipse**-et!
2. Készítsünk egy új **WebTest projektet** a varázsló segítségével!
3. A projekt **webtest** könyvtárába készítsünk egy új **.wt** kiterjesztésű fált a megfelelő tartalommal!
4. Mentsük el a **.wt** kiterjesztésű fájlt! Ekkor automatikusan előáll a neki megfelelő JUnit tesztet tartalmazó Java kód.
5. Kattintsunk jobb gombbal a projekten, és válasszuk a **Run As > JUnit Test** menüpontot. Ennek hatására a JUnit teszt egy Selenium által vezérelt böngészőn keresztül végrehajtja a WebTest nyelven leírt utasításainkat.

## Feltöltendő

Ha ezt a részfeladatot sikerült megoldani, készítsetek screenshot-okat és töltsétek fel a képeket a saját repótokon belül a **homeworks/hw2** mappába az alábbiakról:

* A **Runtime Eclipse**-ben a projekt varázsló által előállított projekt teljes mappaszerkezete kibontva úgy, hogy a varázsló által előállított összes fájl látható legyen.
* A **Runtime Eclipse**-ben a varázsló által készített projekt **webtest** mappájában elkészített legalább 20 soros **.wt** kiterjesztésű fájl, amely mindkét megvalósítandó bővítményre tartalmaz példát.
* A **.wt** fájlból generált Java kód a **Runtime Eclipse**-ben megnyitva.
