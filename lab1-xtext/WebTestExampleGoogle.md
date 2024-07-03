# Példa: Google keresés

A WebTest nyelv különböző utasításokat tartalmaz. Az utasítások segítségével lehet leírni a tesztek forgatókönyveit.

Példa egy egyszerű Google keresésre a WebTest nyelven:
```
webtest example.Google

open "https://www.google.com"
fill textarea "q" with "jwst"
click button "Search"
wait 5 seconds
```

A **webtest** kulcsszó egy Java osztályt definiál, ahová a keletkező JUnit teszt fog kerülni, jelen esetben ez az *example.Google* osztály.

Az **open** kulcsszóval lehet megnyitni a *https://www.google.com* URL-t.

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

// open "https://www.google.com"
driver.navigate().to("https://www.google.com");

// fill textarea "q" with "jwst"
var textareaQ = By.xpath("//textarea[@name='q']");
wait.until(ExpectedConditions.presenceOfElementLocated(textareaQ));
driver.findElement(textareaQ).sendKeys("jwst");

// click button "Search"
var buttonSearch = By.xpath("//button[contains(text(),'Search')]");
wait.until(ExpectedConditions.presenceOfElementLocated(buttonSearch));
driver.findElement(buttonSearch).click();

// wait 5 seconds
Thread.wait(5000);

driver.quit();
```

A Selenium kód megnyitja a Chrome böngészőt és kiteszi teljes képernyőre. Mivel egy weboldal betöltése sok időt vehet igénybe, nem lehet azonnal elkezdeni kiadni az utasításokat. Meg kell várni, amíg megjelennek a weboldal egyes elemei. Ehhez a *FluentWait* osztály ad segítséget: beállítjuk, hogy maximum 10 másodpercig várjon, és másodpercenként ellenőrizze a feltételek teljesülését. Ezután megnyitjuk a *https://www.google.com* weboldalt, majd megvátjuk, amíg a keresési szövegdoboz megjelenik. Ezután begépeljük a szövegdobozba a *jwst* szöveget, majd megnyomjuk a *Search* feliratú gombot. Végül 5 másodpercet várunk, hogy a keresési találatokat megnézhessük, majd bezárjuk a böngészőt.

Egy weboldal elemeit (gombok, szövegdobozok, stb.) a *WebDriver* segítségével érhetjük el a *findElement* függvényen keresztül. A függvénynek meg kell adni egy keresési feltételt, amelyet a *By* osztály ír le. A keresési feltétel sokfajta lehet. A részleteket a [Selenium](https://www.selenium.dev/documentation/webdriver/elements/locators/) dokumentációjában megtalálhatjuk.

Megjegyzés: sajnos a fenti kód nem teljesen működik a tényleges Google keresőoldalon, mert a keresés előtt még a cookie-k használtatát is jóvá kellene hagyni, ami a könnyebb érthetőség kedvéért kimaradt a fenti példából. Mindenesetre a lényeg kiolvasható: jól látható, hogy a WebTest nyelv egy kényelmes absztrakciót biztosít a Selenium API felett. Egy teljes működő megoldás itt látható:

```
webtest example.Google

open "https://www.google.com"

element acceptCookies = button "L2AGLb"
if acceptCookies exists then
    click acceptCookies
end

fill textarea "q" with "jwst"
click button "Search"
wait 5 seconds
```
