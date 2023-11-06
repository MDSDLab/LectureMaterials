# Blockly4HTML nyelv specifikáció

Az alábbiakban a nyelv céljának ismertetése után a nyelvi elemei kerülnek felsorolásra és rövid bemutatásra. 
A felsorolás a [bővítményeket](extensions.md) nem, csak a közös, minden csapat által megvalósítandó nyelvi elemeket tartalmazza. 

## Áttekintés
A nyelv célja, hogy működő weboldalakat lehessen előállítani programozói, ill. HTML ismeretek nélkül. 

### Alapvető építőelemek (Basic blocks)
* **WebPage**
  * A nyelv mindig pontosan egy weboldal (WebPage) definícióját tartalmazza (szerepel a kiadott [példában](init_example.md))
  * Attribútumok:
    * Title (szöveges): a HTML weboldal title eleme
  * Gyermekelemek:
    * Style: a weboldalra (body-ra) érvényesítendő CSS class neve (érték alapú kapcsolat!)
    * Elements: a tartalmazott HTML elemek (az oldal tartalma)
* **List**
  * HTML lista megadására szolgáló elem
  * Attribútumok:
    * Type (legördülő, vagy boolean): ordered (&lt;ol&gt;), unordered (&lt;ul&gt;)
  * Gyermekelemek:
    * Children: a tartalmazott HTML elemek
* **ListItem**
  * A listaelemek megadására szolgáló elem, az elemből egy listaelem (&lt;li&gt;) készül
  *  Gyermekelemek:
      * Children: a tartalmazott HTML elemek
 * **TextBlock**
   * Egy szövegblokk (&lt;p&gt;) megadására szolgáló elem.
   * Attribútumok:
      * Type (legördülő): h1/h2/bold/deleted (&lt;del&gt;) stílust lehet megadni a tartalmazott elemeknek
   * Gyermekelemek:
     * Children: a blokk tartalmát képző HTML elemek, amikre a megadott formázás érvényesül
  * **Link**
    * Egy link megadására szolgáló elem
    * Attribútumok:
      * Text (szöveges): a weboldalon megjelenő szöveg (amire rá lehet kattintani)
      * Href (szöveges): a weboldal, amire a link mutat (ahova a link vezet)
  * **Text**
    * Szöveg megadására szolgáló elem
    * Attribútumok:
      * Text (szöveges): a megjelenítendő szöveg
  * **Div**
    * HTML blokkot (&lt;div&gt;) megadó elem
     * Gyermekelemek:
        * Style: a blokk stílusa (érték alapú kapcsolat!)
        * Children: a blokk tartalmát képző HTML elemek, amikre a megadott formázás érvényesül        
 * **CSS**
    * Egy CSS class-t reprezentáló elem (hivatkozás egy meglévő CSS osztályra)
    * Attribútumok:
      * Name (szöveges): a Css osztály neve

### Bevitel (Input)
* **Form**
  * Beviteli mezőket összefogó űrlap (Form)
  * Attribútumok:
    * Action (szöveges): az űrlap elküldésekor végrehajtandó feladat (a cél URL)
  * Gyermekelemek:
    * Children: a tartalmazott HTML elemek
*  **Input**
   * Beviteli mezőt (input) megadó elem
   * Attribútumok:
     * Type (legördülő): a mező típusa: Password/Text/Checkbox/Number
     * ID (szöveges): a mező azonosítója
     * Caption (szöveges): a mező felirata
   * Gyermekelemek:
     * Validator: a mezőhöz kapcsolt validátor (érték alapú kapcsolat)
 * **Button**
   * Gomb megadására szolgáló elem
   * Attribútumok:
     * ID (szöveges): a gomb azonosítója
     * Label (szöveges): a gomb felirata
     * Action (szöveges): a gombra kattintáskor lefutó függvény (a függvény minden esetben paraméterek nélküli). Ha üres a mező, akkor submit gombként működik az elem.
        
### Validáció (Validation)
* **Required**
    * Megadja, hogy a kapcsolt elem kötelező
* **MinLength**
    * Megadja a kapcsolt elem minimum hosszát
    * Attribútumok:
      * Value (szám): a minimum hossz (0..255 közt adható meg)
* **Pattern**
    * Megadja a kapcsolt elemtől elvárt mintát
    * Attribútumok:
      * Value (szöveges): az elvárt minta reguláris kifejezésként
* **ValidatorMsg**
    * Lehetővé teszi, hogy saját validációs hibaüzenetet adhassunk meg
    * Attribútumok:
      * Message (szöveges): a megjelenítendő validációs hibaüzenet
   * Gyermekelemek:
     * Validator: a testreszabandó validátor (érték alapú kapcsolat)
 
        
### Megkötések
   
   A szerkesztőben a következő megkötéseket kell megadni:
   * A Style mezőkhöz csak CSS típusú elem köthető be
   * A List elemeken belül kezvetlenül csak ListItem elemek lehetnek (de azokon belül bármi, természetesen)
   * A ValidatorMsg elem Validator mezőjébe csak Validator típusú elem (Required, MinLength, Pattern) köthető be
   * Az Input elem Validator mezőjébe csak Validator típusú elem, vagy ValidatorMsg köthető be

A feladatok megoldásában segíthet néhány [tipp](hints.md), ha elakadnátok
