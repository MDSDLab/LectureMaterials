# Szemantikai elemzés

A [mögöttes adatstruktúra](DataStructure.md) felhasználásával már elvégezhető a teljes szemantikai elemzés. A **main.validation.WebtestInputSemanticAnalyzer** segítségével - a visitor mintát alkalmazva - a következő feladatokat kell megoldani; minden egyes esetre saját szemantikai hibát kell adni (**main.validation.WebtestInputValidationErrorHandler**). Néhány ellenőrzést meg lehetne oldani úgy, hogy a nyelvtant nekik megfelelően alakítjuk ki, viszont mindenképpen szeretnénk a következő esetekre saját hibaüzenetet kapni, ezért itt **nem elfogadható megoldás a nyelvtan szabályok alakításával** megoldani a lentieket, mindenképpen saját hibaüzenetet kell adni!

- *minlength*, *maxlength*, és *pattern* típusú HTML attribútum validáció csak *text* típusú input mezőben lehet
- *min* és *max* típusú HTML attribútum validáció csak *number* és *date* típusú input mezőben lehet
- *step* típusú HTML attribútum validáció csak *number* típusú input mezőben lehet
- összehasonlításos validáció (*eq*, *neq*, *lt*, *gt*) *checkbox* típusú input mező esetén nem értelmezett
- összehasonlításos validáció esetén a paraméter típusának meg kell egyeznie az adott *input* típusával
- minden esetben, amikor másik input mezőre (id alapon) hivatkozunk, a referált input mezőnek léteznie kell
- láthatóság szabályozás (*show*, *hide*) csak *checkbox* típusú input mezőben lehet
- láthatóság szabályozásánál nem lehet egymásnak ellentmondó logika (pl. "on? show fname" és "off? show fname")