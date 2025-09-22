# Példa: DuckDuckGo keresés

A WebTest nyelv különböző utasításokat tartalmaz. Az utasítások segítségével lehet leírni a tesztek forgatókönyveit.

Példa egy egyszerű DuckDuckGo keresésre a WebTest nyelven:
```
webtest example.Search

open "https://duckduckgo.com/"
fill input "q" with "jwst"
click button "Search"
wait 5 seconds
```

A **webtest** kulcsszó egy Java osztályt definiál, ahová a keletkező JUnit teszt fog kerülni, jelen esetben ez az *example.Search* osztály.

Az **open** kulcsszóval lehet megnyitni a *https://duckduckgo.com/* URL-t.

A **fill** kulcsszó a *q* azonosítójú szövegdobozba begépeli a *jwst* szöveget.

A **click** kulcsszó megnyomja *Search* feliratú gombot.

A **wait** kulcsszó várakozik 5 másodpercig, hogy meg tudjuk nézni a keresési eredményeket.

A fenti példa az alábbi módon fordítható le Java alapú Selenium kódra, amely a Chrome böngészőn keresztül hajtja végre az utasításokat:

```Java
var driver = new ChromeDriver();
driver.manage().window().maximize();

var wait = new FluentWait<WebDriver>(driver);
wait.pollingEvery(Duration.ofSeconds(1));
wait.withTimeout(Duration.ofSeconds(10));

// open "https://duckduckgo.com/"
driver.navigate().to("https://duckduckgo.com/");

// fill input "q" with "jwst"
var inputQ = By.xpath("//input[@name='q']");
wait.until(ExpectedConditions.presenceOfElementLocated(inputQ));
driver.findElement(inputQ).sendKeys("jwst");

// click button "Search"
var buttonSearch = By.xpath("//button[contains(text(),'Search')]");
wait.until(ExpectedConditions.presenceOfElementLocated(buttonSearch));
driver.findElement(buttonSearch).click();

// wait 5 seconds
Thread.wait(5000);

driver.quit();
```

A Selenium kód megnyitja a Chrome böngészőt és kiteszi teljes képernyőre. Mivel egy weboldal betöltése sok időt vehet igénybe, nem lehet azonnal elkezdeni kiadni az utasításokat. Meg kell várni, amíg megjelennek a weboldal egyes elemei. Ehhez a *FluentWait* osztály ad segítséget: beállítjuk, hogy maximum 10 másodpercig várjon, és másodpercenként ellenőrizze a feltételek teljesülését. Ezután megnyitjuk a *https://duckduckgo.com/* weboldalt, majd megvátjuk, amíg a keresési szövegdoboz megjelenik. Ezután begépeljük a szövegdobozba a *jwst* szöveget, majd megnyomjuk a *Search* feliratú gombot. Végül 5 másodpercet várunk, hogy a keresési találatokat megnézhessük, majd bezárjuk a böngészőt.

Egy weboldal elemeit (gombok, szövegdobozok, stb.) a *WebDriver* segítségével érhetjük el a *findElement* függvényen keresztül. A függvénynek meg kell adni egy keresési feltételt, amelyet a *By* osztály ír le. A keresési feltétel sokfajta lehet. A részleteket a [Selenium](https://www.selenium.dev/documentation/webdriver/elements/locators/) dokumentációjában megtalálhatjuk.

