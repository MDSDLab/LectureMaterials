# Laboratory 3 - VIATRA Modell query and model transformation

## Steps to solve the lab

0. Install VIATRA in Eclipse (Help -> Eclipse Marketplace... -> Find: Viatra -> VIATRA 2.9.1 -> Install)
1. Solve the [VIATRA](https://github.com/ftsrg-edu/lecture-notes/wiki/2020_vql) and [Model transformation](https://github.com/ftsrg-edu/lecture-notes/wiki/2021_m2m) tutorials, or the [official VIATRA](https://eclipse.dev/viatra/documentation/tutorial.html) tutorial.
2. Copy the **webtest.transformation** project from the [mdsd-2024-lab3-viatra](https://github.com/MDSDLab/mdsd-2024-lab3-viatra) repository into your own repository next to the already existing projects starting with **webtest**.
3. Solve the tasks listed below!
5. Add the **hw3** git tag to your last commit!

## Creating queries and transformations for the WebTest language

The goal of the alb is to create queries and model transformations for the WebTest language using VIATRA. The queries are intended to perform various checks on the models of the WebTest language. The model transformations are intended to make various changes to the models of the WebTest language for [mutation testing](https://en.wikipedia.org/wiki/Mutation_testing) purposes.

## List of the tasks

To solve the lab, open the following project in Eclipse:

* **webtest.transformation**

Example:

0. [Try the example](Example.md)

Tasks:

1. [Creating queries](Viatra.md)
2. [Creating Transformations](Transformation.md)

## Check the solution

The generated test cases in the **QueryTests.xtend** file in the **webtest.transformation.querytest** package within the **test** folder help the development of the queries. (Running these tests is mandatory, please use exactly the query names we provided.) However, these tests do not cover all cases, and the issued models do not necessarily contain an example for every possible match of every query. For a more thorough evaluation of the solution to the lab, further tests will be run after submission!


