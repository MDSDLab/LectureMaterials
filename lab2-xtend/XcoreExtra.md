# TIPP: Extra információk hozzákötése az Xcore metamodellhez

A névelemzéshez, típuselemzéshez és egyéb ellenőrzésekhez szükség lehet olyan extra információk számítására és tárolására, amelyek leírása Xcore-ban kényelmetlen. Ilyenkor célszerű ezeket az extra információkat az Xcore metamodelltől leválasztva kiszámolni, majd az eredményt hozzákötni a metamodellhez.

Itt egy lehetséges megközelítést, egy hasznos tippet mutatunk be ennek a megvalósítására. Nem kötelező ezt a megközelítést választanotok, más módszerrel is megoldhatjátok a feladatot.

Nyissátok meg a **webtest.model** projektet, és az **src** könyvtárban a **webtest.model** csomagban hozzátok létre az alábbi **ModelInfo** osztályt Xtend szintaxissal a **ModelInfo.xtend** fájlban:

```
package webtest.model

import java.util.LinkedHashMap

class ModelInfo {
    Main _main
    LinkedHashMap<Variable, VariableInfo> _variableInfo
    
    new (Main main) {
        _main = main
        _variableInfo = new LinkedHashMap
    }
    
    def getMain() { _main }
    
    def VariableInfo getVariableInfo(Variable variable) {
        var info = _variableInfo.get(variable)
        if (info === null) {
            info = new VariableInfo(this, variable)
            _variableInfo.put(variable, info)
        }
        return info
    }
}
```

Ebből a kódból az Xtend egy **ModelInfo** nevű Java osztályt generál a projekt **xtend-gen** könyvtárában. Az osztály ugyanúgy hívható, mint minden más Java osztály, az Xtend nyelv mindössze csak leegyszerűsíti a Java szintaxisát. Az Xtend nyelvről további információk [itt találhatók](https://eclipse.dev/Xtext/xtend/documentation/index.html).

Hozzatok létre egy **VariableInfo** osztályt is a **webtest.model** csomagban:

```
class VariableInfo {
    ModelInfo _modelInfo
    Variable _variable
    
    new (ModelInfo modelInfo, Variable variable) {
        _modelInfo = modelInfo
        _variable = variable
    }
    
    def getType() {
        if (_variable.value !== null) return _variable.value.^type
        else return Type.UNDEFINED // TODO
    }
}
```

A **model** könyvtárban a **WebTest.xcore** fájlban végezzétek el az alábbi kiegészítéseket:

```
package webtest.model

type ModelInfo wraps ModelInfo
type VariableInfo wraps VariableInfo

class Main
{
    unsettable ModelInfo infoLazy
    String[] testClass
    contains Declaration[] declarations
    contains BlockStatement body
    
    op ModelInfo getModelInfo() {
        if (infoLazy === null) infoLazy = new ModelInfo(this)
        return infoLazy
    }
}

abstract class NamedElement
{
    String name
    
    op ModelInfo getModelInfo() {
        org.eclipse.xtext.EcoreUtil2.getContainerOfType(this, Main).modelInfo
    }
}

class Variable extends NamedElement
{
    unsettable VariableInfo infoLazy
    contains Expression value

    op VariableInfo getInfo() {
        if (infoLazy === null) infoLazy = modelInfo?.getVariableInfo(this)
        return infoLazy
    }
    
    op Type getType() {
        if (info !== null) return info.^type
        else return Type.UNDEFINED 
    }
}
```

A fenti módosítások eredménye a következő:

* A **Main** objektumhoz, ami a modell gyökere, pontosan egy **ModelInfo** objektum lesz hozzácsatolva.
* Minden **NamedElement**-ből elérhető ez a **ModelInfo** objektum: az **EcoreUtil2.getContainerOfType** függvénnyel megkeressük a **NamedElement**-et tartalmazó modell gyökerét, és attól elkérjük a **ModelInfo** objektumot.
* A **Variable** osztályban lekérdezzük az adott változóhoz tartozó **VariableInfo** információt, amelyet a **ModelInfo** tárol. Ettől a **VariableInfo** objektumtól pedig lekérdezzük a változó számított típusát.
* A **VariableInfo** osztályban a **getType()** függvényben kell kiszámítani a változó típusát. Ha a változónak van kezdőértéke, akkor a típus onnan lekérdezhető. Egyébként ki kell következtetni a típust a típuselemzésben megadott elvárt típusok alapján. Ez a típuskövetkeztetés (TODO ág) már a laborfeladat megoldásának a része, a fenti kód csak a vázat biztosítja hozzá.

A fentiekhez hasonlóan csatolhattok extra információkat a kifejezésekhez (pl. **ExpressionInfo**) és az utasításokhoz is (pl. **StatementInfo**), vagy bármely egyéb elemhez, amihez szükségesnek látjátok. Ezek az extra információk segíthetnek az Xcore-ban nehezen leírható dolgok kiszámításában.
