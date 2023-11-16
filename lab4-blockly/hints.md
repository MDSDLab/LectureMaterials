# Segítségek, tippek

 * A feladat megoldása során nagyon sokat segíthet a Blockly [dokumentáció](https://developers.google.com/blockly/guides/overview)
 * A Szövegblokk esetén alapból &lt;p&gt; tag-et kell generálni, de ha a Type attribútum meg van adva, akkor ehelyett más tag-re van szükség (pl. &lt;h1&gt;), hiszen a &lt;p&gt; tag nem tartalmazhatna blokkelemeket.
 * A BlockFactory által generált generátor-csonk nem minden Blockly verzióval fordul le közvetlenül, ha ez a hiba előjönne, akkor a Blockly.javascript névteret érdemes átírni Blockly.JavaScript-ra
 * A beépített validáció, ami az egyes elemek összekapcsolásakor fut le csak akkor működik, ha mindkét oldalon be van állítva valamilyen érték. A kapcsolódó elemeknél érdemes emiatt a setOuput függvénnyel beállítani a típust (pl. validator)
 * Az egyes blokkoknál nagyon sokrétűen használható a this.setOnChange függvény, amivel pl. saját validációs logikát adhatunk meg
 * A validációnak két szintje van, a warning és a tényleges hiba, ezek a setWarningText és a setEnable fv-ekkel hívhatóak meg
 * A Blockly dokumentációban is szereplő módon az egyes blokkoktól lekérdezhetőek a bennük definiált field-ek, ill. inputok (getField/getInput). Arra érdemes figyelni, hogy ahol két blokk kapcsolódik (pl. egy függvény paramétert kap), ott gyakran a connection property-t érdemes használni ezek helyett
 * A Blockly API sokat segít a meglévő elemek eltávolításában, vagy új elemek hozzáadásában is (pl. removeField/appendField)
 * A validátorok esetében elegendő a beépített HTML5 validáció. Az egyéni hibaüzeneteknél a setCustomValidity JS függvény használata javasolt ([példa](https://stackoverflow.com/questions/5272433/how-to-set-custom-validation-messages-for-html-forms)), így nem kell külön Javascript fájlt is generálni. Érdemes viszont figyelni rá, hogy a validáció a Submit-re fut csak le. Nem kell ellenőrizni, hogy van-e Submit gomb elhelyezve a Formon.
