# Blockly4HTML nyelv specifikáció

Az alábbiakban a nyelv céljának ismertetése után a nyelvi elemei kerülnek felsorolásra és rövid bemutatásra. 
A felsorolás a [bővítményeket](extensions.md) nem, csak a közös, minden csapat által megvalósítandó nyelvi elemeket tartalmazza. 

## Áttekintés
A nyelv célja, hogy működő weboldalakat lehessen előállítani programozói, ill. HTML ismeretek nélkül. 

### Alapvető építőelemek (Basic blocks)
* **WebPage**
  * A nyelv mindig pontosan egy weboldal (WebPage) definícióját tartalmazza (maga a Webpage szerepel a kiadott [példában](init_example.md))
  * A szerkesztési térben alapból jelenjen meg (ne kelljen manuálisan hozzáadni)!
  * Attribútumok:
    * Title (szöveges): a HTML weboldal title eleme
  * Gyermekelemek:
    * Elements: a tartalmazott HTML elemek (az oldal tartalma)
* **Table**
  * Táblázat megadására szolgáló elem
  * Gyermekelemek:
    * Rows: a táblázat sorai 
* **Row**
  * A táblázat egy sorát adja meg
  *  Gyermekelemek:
	  * Height: a sor magasságát adja meg (érték alapú kapcsolat)
      * Cell: a sorban található cellák
* **Cell**
  * A táblázat egy celláját adja meg
  *  Gyermekelemek:
	  * Width: a cella szélességét adja meg (érték alapú kapcsolat)
	  * Children: a cella tartalmát képző HTML elemek	  
* **Text**
    * Egy szövegblokk (&lt;p&gt;)megadására szolgáló elem.
    * Attribútumok:
      * Text (szöveges): a megjelenítendő szöveg  
 * **HeaderText**
   * Egy címszöveg (pl. &lt;h&gt;) megadására szolgáló elem.
   * Attribútumok:
      * Type (legördülő): h1/h2/h3/h4 stílust lehet megadni a tartalmazott elemeknek
	  * Text (szöveges): a megjelenítendő szöveg  
  * **Image**
    * Egy kép beszurására szolgáló elem
    * Attribútumok:
      * Href (szöveges): a weboldal, ahol a kép megtalálható
	* Gyermekelemek:
		* Width: a kép szélessége (érték alapú kapcsolat!)
		* Height: a kép magassága (érték alapú kapcsolat!)
  * **Div**
    * HTML blokkot (&lt;div&gt;) megadó elem
     * Gyermekelemek:
		* Width: a blokk szélessége (érték alapú kapcsolat!)
		* Height: a blokk magassága (érték alapú kapcsolat!)
        * Padding: a blokk belső margója (egységes minden irányban) (érték alapú kapcsolat!)
        * Children: a blokk tartalmát képző HTML elemek, amikre a megadott formázás érvényesül        
  * **Size**
     * Megad egy hosszúság, vagy szélesség értéket (pl. 25px)
	 * Attribútumok:
	    * Amount (szöveges): a konkrét érték (pl. 25)
	    * Type (legördülő): px (képpont), vagy % (százalék)


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
   * Az űrlap beküldésére szolgáló elem
   * Attribútumok:
     * Label (szöveges): a gomb felirata
        
### Validáció (Validation)
* **Required**
    * Megadja, hogy a kapcsolt elem kötelező
* **MaxLength**
    * Megadja a kapcsolt elem maximum hosszát
    * Attribútumok:
      * Value (szám): a maximum hossz (100..1000 közt adható meg)
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
   * A Width/Height/Padding mezőkbe csak Size típusú elem köthető be
   * A Table elemeken belül követlenül csak Row elemek lehetnek, a Row-kon belül csak Cell-ek (tehát pl. Input nem)
   * A ValidatorMsg elem Validator mezőjébe csak Validator típusú elem (Required, MinLength, Pattern) köthető be
   * Az Input elem Validator mezőjébe csak Validator típusú elem, vagy ValidatorMsg köthető be

A feladatok megoldásában segíthet néhány [tipp](hints.md), ha elakadna valaki.