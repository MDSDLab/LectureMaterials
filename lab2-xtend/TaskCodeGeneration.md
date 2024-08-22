# Kódgenerálás (code generation)

A kódgenerálás során a feldolgozott és leellenőrzött WebTest kódokból JUnit teszteket kell automatizáltan előállítani. Lényegében a WebTest kódokat Java kódra kell lefordítani.

A generált Java kódoknak a [projekt varázslóban](TaskProjectWizard.md) létrehozott projektstruktúrába kell illeszkednie, és azon belül kell működőképesnek lennie.

A kódgenerálás megvalósítására legcélravezetőbb az [Xtend](https://eclipse.dev/Xtext/xtend/documentation/index.html) nyelv és az abban található [sablonok](https://eclipse.dev/Xtext/xtend/documentation/203_xtend_expressions.html#templates) használata, így ebben a részfeladatban célszerű **xtend** kiterjesztésű fájlokat készíteni.

## A generálás előkészítése

Mielőtt nekilátunk egy kódgenerátor írásának, először célszerű manuálisan megírni azokat a kódokat, amiket szerenétek automatizáltan előállítani. Ezek példaként szolgálhatnak a generátor sablonjainak elkészítéséhez. 

Szerencsére ezeket a példakódokat megírtuk helyettetek a **webtest.example** projektben, amelyben mind a WebTest kódok, mind a belőlük előállítandó Java kódok megtalálhatók. Célszerű, de nem szükséges pontosan ugyanezeket a Java kódokat előállítani. Elegendő, ha viselkedésben megegyezik az általatok generált Java kód a példaként megadott kódokkal.

Vizsgáljátok meg a **webtest.example** projektben található WebTest kódokat, a belőlük előálló JUnit teszteket, és a tesztek alapját biztosító segédosztályokat (**Page**, **PageElement**, **SeleniumTest**)!

A [projekt varázslóban](TaskProjectWizard.md) már elkészítettük nektek a projekt struktúráját, és beépítettük a segédosztályok (**Page**, **PageElement**, **SeleniumTest**) generálását is. Ezekhez ne nyúljatok hozzá, ezeken ne módosítsatok! (Kivéve esetleg a **DEFAULT_CHROME_DRIVER_LOCATION** konstans értékén a generált **SeleniumTest.java** osztályban, ha szükséges.)

Ebben a részfeladatban csak annyi a dolgotok, hogy minden egyes WebTest fájlból pontosan egy Java fájlt hozzatok létre, amely a WebTest fájlnak megfelelő JUnit tesztet reprezentálja.

## A generátor váza

Segítségképpen megadjuk a generátor vázát, amiből könnyebben ki lehet indulni a feladat megoldása során. Hozzatok létre a **webtest.generator** projekten belül az **src** könyvtár alatt a **webtest.generator** csomagban egy **UnitTestGenerator.xtend** fájlt az alábbi tartalommal:

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

Ez egy minimális JUnit tesztvázat tud generálni, amelyben az egyes függvények törzse még nincs kitöltve. A *TODO* helyekre írhatjátok a saját kódotokat, de nyugodtan hozzányúlhattok a fenti kód többi részéhez is, ha úgy látjátok célszerűnek. Sőt, egyes bővítmények esetén hozzá is kell nyúlni. A fenti példa célja mindössze az, hogy demonstrálja az Xtend nyelv képességeit, és ötleteket adjon az elinduláshoz.

Ahhoz, hogy az Xtext figyelembe vegye a generátorotokat, meg kell hívni a generátort a **webtest.dsl** projekten belül található **webtest.dsl.generator.WebTestDslGenerator** osztályból. Módosítsátok az osztályt az alábbi módon:

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

Ez a generátor a **resource** paraméterből előbányássza a WebTest modellünk **Main** gyökerét, átadja a generátorotoknak, majd az eredményként előálló Java kódot az **src-gen/test/java** könyvtárban helyezi el.

## Feladat

Módosítsátok a **UnitTestGenerator** osztályt, hogy teljesen működő JUnit teszteket állítson elő!

A megvalósítandó 2 bővítmény támogatását se felejtsétek el beépíteni a generátorba!

***TIPP:** Az Xtend nyelv [dispatch](https://eclipse.dev/Xtext/xtend/documentation/202_xtend_classes_members.html#polymorphic-dispatch) függvénydeklarációja hasznos lehet az utasítások és kifejezések generálása során, amelyet meghívva futásidőben a dinamikus típus alapján dől el, hogy melyik overload változat hívódik meg, így nem kell **instanceof**-ot használni.*

***TIPP:** Ha a **Runtime Eclipse**-et Debug módban indítjátok, és nem végeztek komoly változtatást a generátor kódjában, akkor a változások automatikusan végrehajtódnak, és nem kell folyton újraindítanotok a **Runtime Eclipse**-et. Elegendő a WebTest kódban egy apró módosítást végezni, és azt elmenteni, hogy a generátor újra lefusson. Az Eclipse szólni fog, ha a generátorban történt változás olyan mértékű, amely újraindítást igényel.*

## Ellenőrzés

Az alábbi módon lehet ellenőrizni a generátor helyes működését:

1. Indítsuk el a **Runtime Eclipse**-et!
2. Készítsünk egy új **WebTest projektet** a varázsló segítségével!
3. A projekt **webtest** könyvtárában készítsünk egy új **.wt** kiterjesztésű fált a megfelelő tartalommal!
4. Mentsük el a **.wt** kiterjesztésű fájlt! Ekkor automatikusan előáll a neki megfelelő JUnit tesztet tartalmazó Java kód.
5. Kattintsunk jobb gombbal a projekten, és válasszuk a **Run As > JUnit Test** menüpontot. Ennek hatására a JUnit teszt egy Selenium által vezérelt böngészőn keresztül végrehajtja a WebTest nyelven leírt utasításainkat.

## Feltöltendő

Ha ezt a részfeladatot sikerült megoldani, készítsetek screenshot-okat és töltsétek fel a képeket a saját repótokon belül a **homeworks/hw2** mappába az alábbiakról:

* A **Runtime Eclipse**-ben a projekt varázsló által előállított projekt teljes mappaszerkezete kibontva úgy, hogy a varázsló által előállított összes fájl látható legyen.
* A **Runtime Eclipse**-ben a varázsló által készített projekt **webtest** mappájában elkészített legalább 20 soros **.wt** kiterjesztésű fájl, amely mindkét megvalósítandó bővítményre tartalmaz példát.
* A **.wt** fájlból generált Java kód a **Runtime Eclipse**-ben megnyitva.
