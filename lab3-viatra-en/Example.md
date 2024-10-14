# Try out the example

In the **webtest.transformation** project, there is an example query in the **queries.vql** file in the **webtest.transformation.queries** package within the **queries** folder. There is an example transformation rule in the **MutationRules.xtend** file in the **webtest.transformation.rules** package within the **src** folder.

In the **example** and **error-example** directories within the **webtest.transformation** project, you can find example models on which you can try out the queries and transformations.

## Running the queries
You can use the **Query Results** view to see the results of the Viatra queries. You can open it by selecting **Window -> Show View -> Other...** in the menu and selecting **Viatra -> Query Results**. To make the queries work correctly, you need to enable **Dynamic EMF mode** in **Window -> Preferences -> VIATRA -> Query Evaluation**.

Open the **calculator-1.xml** file in the **error-examples** folder with the **Sample Reflective Ecore Model Editor**, click on the green triangle in the top right corner of the Query Results view (**Load model from active editor**) to load the model, then open the **queries.vql** file and click on the newly colored green triangle (**Load queries from active editor**) to load the queries. The result of the query will appear in the Query Results view. (If the result does not appear or does not update, edit and save the vql file, for example by inserting an empty line, and immediately click the **Load queries from active editor** button.)

Nyissátok meg az error-examples mappában található **calculator-1.xml** fájlt a **Sample Reflective Ecore Model Editor**-ral, és a Query Results nézet jobb felső sarkában kattintsatok a zöld háromszögre (**Load model from active editor**) a modell betöltéséhez, majd nyissátok meg a **queries.vql** fájlt, és kattintsatok az újonnan kizöldült háromszögre (**Load queries from active editor**) a lekérdezések betöltéséhez. A lekérdezés eredménye megjelenik a Query Results nézetben. (Ha mégsem jelenik meg vagy nem frissül az eredmény, szerkesszétek és mentésétek a vql fájlt, például egy üres sor beszúrásával, majd egyből kattintsatok a **Load queries from active editor** gombra.)

## Running query tests
There are some generated tests in the **QueryTests.xtend** file in the **webtest.transformation.querytest** package within the **test** folder. To run these tests, use the **Right click -> Run As -> JUnit Test** menu item.

## Running transformations
You can try out the transformations by running the **MutationRules.xtend** file: **Right click -> Run as -> Java Application**. This loads the specified model, performs N randomly selected transformations (default is 2), and saves the modified model to the specified folder. By default, this is the **output** folder, and if you run the application again, it will overwrite the previous mutation. If you do not see any changes in the opened model, refresh the file.