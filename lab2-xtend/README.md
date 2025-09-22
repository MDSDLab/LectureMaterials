# 2. labor - IDE támogatás és Xtend kódgenerátor

## A laborfeladat elvégzésének lépései

0. Ezen a laboron az első laboron megoldott feladatot kell majd tovább folytatni.
1. Csináljátok végig az [Xtend](https://eclipse.dev/Xtext/documentation/103_domainmodelnextsteps.html) tutorialt!
2. Oldjátok meg az alábbiakban leírt feladatokat, figyelembe véve a számotokra kiosztott bővítményeket!
3. Beadás előtt ellenőrizzétek, hogy minden projekt hiba nélkül fordul-e a repóbol tisztán kiszedve is.
4. Készítsetek egy **hw2** nevű git tag-et az utolsó commitra!

## Feladat célja

A laborfeladat célja Xtext segítségével Eclipse IDE támogatás, valamint Xtend segítségével kódgenerátor készítése a WebTest nyelvhez. Az IDE támogatás célja, hogy leegyszerűsítse a WebTest fájlok szerkesztését. A kódgenerátor pedig a WebTest fájlokból JUnit teszteket állít elő úgy, hogy azokat közvetlenül futtatni lehessen.

A WebTest nyelv közös részét mindenkinek meg kell valósítania. A bővítmények közül csak a kiosztott 2 darab bővítményt kell kötelezően támogatni, a többi bővítmény támogatása opcionális.

## Feladat leírása

Az előző (**lab1-xtext**) labor eredményét kell továbbfejleszteni. Nyissátok meg a **webtest-xtext-xtend** könyvtár alatt található projekteket Eclipse alatt!

Oldjátok meg az alábbi feladatokat:

1. [Kódvázlat (outline view, label provider)](TaskOutline.md) (5 pont)
2. [Kódszínezés (syntax coloring, highlighting)](TaskHighlighting.md) (5 pont)
3. [Projekt varázsló (project wizard)](TaskProjectWizard.md)
4. [Kódgenerálás (code generation)](TaskCodeGeneration.md) (15 pont)

## Megoldás ellenőrzése

A **webtest.dsl.ui.tests** projekt azt teszteli, hogy az elkészített Xtext IDE támogatás megfelel-e a laborfeladat követelményeinek.

A laborfeladat megoldása akkor tekinthető késznek, ha az összes releváns teszt sikeresen le tud futni, és a generátor is megfelelően működik. Azonban a laborfeladat megoldásának alaposabb kiértékeléséhez a itt szereplő (előre kiadott) teszteken túl további tesztek is futtatásra kerülhetnek a leadás után!

Ha az összes részfeladattal végeztetek, az alábbi működés az elvárt:

1. Indítsuk el a **Runtime Eclipse**-et
2. Készítsünk egy új **WebTest projektet** a varázsló segítségével
3. A projekt **webtest** könyvtárában készítsünk egy új **.wt** kiterjesztésű fált
4. Írjunk WebTest nyelven teszteket a **.wt** kiterjesztésű fájlba, és közben élvezzük a saját Xtext alapú IDE támogatásunk előnyeit: kódszínezés, outline nézet, validációk, stb.
5. Mentsük el a **.wt** kiterjesztésű fájlt. Ekkor automatikusan előáll a neki megfelelő JUnit tesztet tartalmazó Java kód.
6. Kattintsunk jobb gombbal a projekten, és válasszuk a **Run As > JUnit Test** menüpontot. Ennek hatására a JUnit teszt egy Selenium által vezérelt böngészőn keresztül végrehajtja a WebTest nyelven leírt utasításainkat.

## Referenciák

Hasznos linkek a feladat megoldásához:

* Az előző féléves Xtext-Xtend gyakorlat [útmutatója](../lab1-xtext/images/GY3-Xtext-Utmutato.pdf)
* [Xtext](https://eclipse.dev/Xtext/documentation/index.html)
* [Xtend](https://eclipse.dev/Xtext/xtend/documentation/index.html)
