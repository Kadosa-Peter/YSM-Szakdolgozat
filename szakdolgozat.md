# YSM - YouTube Subscription Manager
## Kadosa Péter SZF47
---

## Alkalmazás Rövid bemutatása

A dolgozatom témája YouTube Subscription Manager, ami egy olyan program ami a YouTube-os feliratkozások menedzselésének megkönnyítése végett jött létre. A dolgozatban a YouTube Subscription Manger-re YSM-ként fogok hivatkozni.

Először is írnék arról, hogy milyen igény hívta életre a YSM-et. Nekem is és még a világon sokaknak a YouTube központi helyett foglal el tartalomfogyasztási szokásaiban. Minden egyes nap több órát töltök a YouTube-on. Néha csak a háttérzenét szolgáltatja a házimunkához, de sokszor a YouTube csiszolom szoftverfejlesztő tudásomat oktató videókat nézve. Természetesen a YouTube-ot független videó készítők tették egy megkerülhetetlen online tényezővé. Sokszor előfordul, hogy egy-egy videós csatornájának tartalma, annyira megtetszik, hogy annak minden új videójára kíváncsiak vagyunk. Erre a problémára ad megoldást az a lehetőség a YouTube-on, hogy feliratkozhatunk csatornákra. És elméletben így mindig kapunk értesítést a YouTube felületén, ha megjelenik új videó az adott csatornán. Viszont ez a lehetőség olyan szegényesen van megvalósítva, hogy a gyakorlatban nem igazán működik. Ha több száz feliratkozásunk van, akkor lehetetlen azoknak az áttekintése, mert a YouTube semmilyen rendezési lehetőséget nem add számunkra. Egyszerűen megjeleníti egy végtelennek tűnő listát a weboldal bal oldalán, ahol egy kék pont jelzi, ha a csatornán van elérhető új videó.

Ennek a problémának a megoldására jött létre a YSM projekt. YSM alapötlete az volt hogy feliratkozásokat téma szerint, kategóriákba lehessen rendezni, valamint hogy a feliratkozásokhoz tartozó videókat a felhasználó a kedve szerint megnézetnek tudja jelölni. A kategóriák és feliratkozások kapcsolatát úgy kell elképzelni, mint az operációs rendszer mappa-fájl kapcsolatát, ahol a mappa a kategória a feliratkozás pedig a fájl.

Mivel mindig is inkább Windows Operációs rendszerre szerettem fejleszteni Microsoft-os technológiákkal, mintsem webre HTML és JavaScript használatával, ezért érthetően a program egy asztali alkalmazás lett, ami C#/WPF segítségével készült el.


## Felhasznált Technológiák

.Net: *.net rövid bemutatása itt...*

C#: *C# rövid bemutatása itt...*

WPF: *WPF rövid bemutatása itt...*

XUnit: *XUnit rövid bemutatása itt...*


## Alkalmazás tervezése

## Felhasználói Élmény UX

Egy jó felhasználói felületnek jól átláthatónak és könnyen kezelhetőnek kell lennie. Nem szabad olyan alkalmazást készíteni, aminek az első indulása után, a felhasználónak, rögtön a kézikönyvhöz vagy oktató videókat kell tanulmányoznia. Persze a fenti megállapítása, nem vonatkozik a mérnöki alkalmazásokra. Úgy vélem a fenti szempontnak a YSM megfelel. Ugyanakkor, az is elmondható, hogy UX szempontjából egy súlyos hibát vétetettem. Mégpedig, hogy nem auditáltam. A YSM tervezését úgy végeztem, hogy inkább a saját igényeimnek feleljen meg mint a jövőbeli felhasználóinak. Például nem implementáltam hozzászólási funkciót a videókhoz. Én sosem írok hozzászólásokat videókhoz, de azt nem lett volna szabad feltételeznem, hogy más YSM felhasználóknak sem lesz ilyen igényük.

## Projekt struktúra, Kódolás

A YSM projekt logikailag jól elhatárolható részeit külön-külön könyvtárakba rendeztem. Például a felhasználói felületet felépítő vezérlőket, és stílusokat a YSM.Controls könyvtárba, az adat-modell osztályokat a YSM.Data könyvtárba. 

*YSM projekt-struktúra bemutatása itt...*

A YSM is használ külső fejlesztők által készített könyvtárakat. Az egyik ilyen a [YoutubeExplode](https://github.com/Tyrrrz/YoutubeExplode), amit Alexey Golub készített. A YSM által használt könyvtárak menedzselését természetesen a Nuget csomagkezelő végzi. Ha a legjobb kódolási gyakorlatok (coding best practices) témában olvasunk, gyakran találkozunk azzal a kifejezéssel, hogy ne találd fel újra a kereket (Do not reinvent the wheel). Vagyis ha valamilyen funkcióra van szükséged, és azt már más elkészítette és szabadon hozzáférhetővé tette, akkor ne tölts el heteket arra, hogy újra elkészítsd ugyanazt a funkciót. Nos, az én tapasztalatom, hogy ez nem mindig a helyes döntés. Ha egy alkalmazás működése nagyban függ egy adott funkciótól, akkor azt a funkciót érdemes saját magunknak implementálni. Ugyanis ha bármilyen külső vagy belső változás miatt a funkció nem lesz működőképes vagy kompatibilis az alkalmazásunkkal, akkor várhatunk az eredeti fejlesztőjére, hogy kijavítsa a hibát vagy megoldja a kompatibilitási problémát. 

A kódolást és a különböző kódolási mintákat (code patterns), elég ad-hoc módon végeztem. Mai fejjel már nem fordulhatna elő, hogy egy projektet ne a [SOLID](https://en.wikipedia.org/wiki/SOLID) és a [KISS](https://en.wikipedia.org/wiki/KISS_principle) (keep it simple, stupid! vagy keep it stupid simple) tervezési alapelv szerint készítsek.


## YSM Logikai és Vizuális Strukturálása

YSM logika felépítése négy, vizuális felépítése három fő részre bontható.

## Logikai Struktúra

## Adat-modellek Tárolása

YSM-ben az adatok tárolásáról SQLite adatbázis gondoskodik. SQLite egy önálló, kompakt és gyors relációs SQL adatbázis. A legnagyobb előnye, hogy nem egy kliens-szerver architektúrájú adatbázis, hanem közvetlenül az alkalmazásból lehet bele adatokat írni, illetve kiolvasni. YSM három fő adatmodelljét egy-egy SQL táblában tárolom. 
[Adatbázis létrehozása C# kódból](https://github.com/Kadosa-Peter/YSM/blob/main/Ysm.Data/Database/Database.cs)

## Feliratkozások 
A felhasználó YouTube feliratkozásait képezi le. A feliratkozásokat a YouTube API segítségével töltöm le.

```sql
CREATE TABLE Subscription(
`Id`	        TEXT,
`SubscriptionId`TEXT,
`Parent`	    TEXT,
`Title`	        TEXT,
`Date`	        TEXT,
`Thumbnail`	    TEXT);
 ```

 A forráskódban a subscriptions táblának channels a neve.

## Kategóriák 
A felhasználó által létrehozott egységek. A felhasználó kategóriákba rendezheti a feliratkozásait. A kategóriákkal fa/hierarchikus adatstruktúrát lehet létrehozni. A kategóriák és feliratkozások kapcsolatát úgy kell elképzelni, mint az operációs rendszer mappa-fájl kapcsolatát, ahol a mappa a kategória a feliratkozás pedig a fájl.

```sql
CREATE TABLE Categories(
`Id`	        TEXT,
`Parent`	    TEXT,
`Title`	        TEXT,
`Color`	        TEXT);
 ```

## Videók 
A feliratkozásokhoz tartozó videók. A YSM időről-időre letölti a feliratkozásokhoz tartozó új videókat a YouTube RSS csatornáján keresztül.

```sql
CREATE TABLE Categories(
`Id`	        TEXT,
`Parent`	    TEXT,
`Title`	        TEXT,
`Color`	        TEXT);
 ```

 ## 2. OAuth2 autentikáció

 A YSM felhasználó YouTube feliratkozásait a YouTube hivatalos API-jának segítségével tölti le. Természetesen ennek első lépése, hogy a felhasználó azonosítsa magát, és engedélyt adjon a YSM-nek, hogy az kezelje a  feliratkozásait. Az engedély kérést OAuth2 segítségével, lehet elvégezni, ami egy ipar által elfogadott megoldás arra, hogy egy harmadik fél hozzáférjen egy alkalmazás erőforrásaihoz. Oauth2 autentikáció végrehajtásához rengeteg szabadon elérhető könyvtár áll rendelkezésünkre. Ugyanakkor mi magunk is megvalósíthatjuk azt, mivel egy igen egyszerű, néhány lépéses folyamatról van szó. Csak arra kell ügyelnünk, hogy kövessük az IETF ide vonatkozó ajánlásait, amik a következők:

 + Az autorizáció soha ne egy beépített WebView-ben, hanem a felhasználó elsődleges böngészőjében valósuljon meg.
 + „State” paraméter alkalmazása [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) támadások elkerülése érdekében.
 + PKCE kiterjesztés alkalmazása több különböző támadás elkerülése érdekében.

 Néhány fontos paraméter leírása az OAuth2 autentikációval kapcsolatban.

**client_id:** 
Az alkalmazás egyedi, publikus kliens azonosítója. Az alkalmazásunk regisztrálása után a Google Console férünk hozzá a kliens azonosítóhoz.

**client_secret:**
Nem publikus biztonsági kulcs a kliens azonosításához. Hasonlóan a client_id-hez, regisztráció után a Google API Console-on találjuk.

**redirect_url:**
Meghatározza, hogy az autentikációs szerver hova írányitsa vissza a felhasználót, az autentikációs folyamat befejeztével. Emlékezzünk, az autorizációnak a felhasználó elsődleges böngészőjében, kell történnie.

**state:**
A state paraméter egy az alkalmazás által generált egyedi biztonsági kulcs. A state paramétert elküldjük az autentikációs szervernek, és azt a szerver visszaküldi. Így elkerülhetünk Cross Site request Forgery támadásokat.

**PKCE:**
PKCE egy biztonsági kiterjesztés OAuth autentikációhoz. A célja, hogy bizonyos támadásokat az autentikációs folyamatban elhárítson. A PKCE két részből áll egy code_verifier-ből ami kriptográfiailag előállított véletlen karakter lánc. És a code_verifier-ből leszármaztatott code_chellenge.

**scopes:**
A scopes paraméterrel szűkíthetjük, azoknak az erőforrásoknak a körét, amihez az alkalmazás hozzáférhet.

A következőkben felvázolom a Google OAuth2 autorizáció lépéseit tömören.

1. Természetesen az első lépés, hogy beregisztráljuk az alkalmazásunkat a Google-nél, és engedélyt kérünk, hogy a jövőben hozzáférhessünk bizonyos erőforrásokhoz (feliratkozások kezelése, videó feltöltése etc.). Regisztrációkor kapjuk meg a client_id-t és a client_secret-et is.

2. Amikor egy felhasználó elindítja az Oauth2 autorizációs folyamatot, akkor az első dolgunk. Hogy a szerkesztünk egy autorizációs url-t. Az autorizációs url-nek a következő paramétereket kell tartalmaznia: client_id, client_secret, code_chellenge, redirect_url, state, scopes

3. Ezt az authorizációs url-t a felhasználó elsődleges böngészőjében kell megnyitni. Itt a felhasználó elfogadja, vagy elutasítja a jogot, hogy hozzáférjen az alkalmazás a scopes paraméterben meghatározott adataihoz

4. Ezután az autorizációs url-ben megadott redirect_url segítségével visszatérünk az alkalmazásba.

5. Ha a felhasználó megadta az engedélyt, akkor a válasz url tartalmazni fog egy „code” paramétert. A „code” paraméterrel kell indítanunk egy 1 egyszerű POST webhívást, aminek a következő paramétereket kell tartalmaznia.
client_id, client_secret, code, code_verifier, grant_type, redirect_url

6. A válaszban immár megkapjuk az access_token-t és a refresh_token-t amivel API hívásokat indíthatunk. Innentől már csak arra kell figyelnünk, hogy az access_token-t időről-időre frissítenünk kell, a refresh_token segítségével.

 ## 3. Feliratozások letöltése

 A feliratkozások letöltése a Google API hívásokkal történik OAuth2 autentikáció után. API hívások egy szerű GET/POST/DELETE webkérések. 

 A feliratkozások letöltéshez a következő GET webkérést kell indítani:

 ```sql
https://www.googleapis.com/youtube/v3/subscriptions?part=snippet%2CcontentDetails&mine=true
  ```

Természetesen a webkérésnek tartalmaznia kell az access_token-t is. A webkérésre kapunk egy JSON választ. A JSON-ban vannak a feliratkozások, maximum 50 darab és egy next_page_token. Ha ötvennél több feliratkozásunk van, akkor indítanunk kell egy új API webkérést, immár a next_page_token paraméterrel. Ekkor kapunk újabb ötven feliratkozást és egy újabb next_page_token-t. Ezeket az API hívásokat addig kell ismételni egy while ciklusban, amíg a next_page_token null érték nem lesz. Ami azt jelenti, hogy megkaptuk az összes feliratkozást. Általánosságban elmondható, hogy ahhoz hogy egy felhasználó összes feliratozását letöltsük több API hívást kell indítanunk.

Általában API hívások száma quota-kal van szabályozva. Az azt jelenti, hogy az alkalmazás/felhasználó egy nap/órában/másodpercben hány API hívást tud indítani. Az elhasznált quota mértéke az API hívás típusától függ, általában a listázás API hívás használja a legkevesebb quota-t amíg beillesztés/törlés/frissítés a legtöbbet.

A feliratkozások letöltésénél a quota felhasználását a következő képen minimalizálom. Az API hívás JSON válaszában minden esetben megkapjuk, hogy összesen hány darab feliratkozása van a felhasználónak. Ez az érték mindig le van mentve. És csak akkor halad végig a while cikklus az API hívásokkal, amikor az új érték eltér a mentett értéktől. 

Amikor az összes feliratkozás letöltődött, utána már csak össze kell hasonlítani, a már korábban letöltött és adatbázisban eltárolt feliratkozásokkal, hogy megtudjuk melyek az új feliratkozások

Természetesen az API hívásokat egy UI-tól külön szálon indítom. A szálkezelésről a [Task Parallel Library](https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/task-parallel-library-tpl) gondoskodik.

## 4. Videók letöltése

A videók letöltése is megvalósítható lenne API hívások segítségével, de az API quota felhasználásának mérséklésének érdekében és egy másik módszert választottam. A YouTube csatornákhoz biztosítva vannak RSS Feed url-ek és a már letöltött feliratkozások adataival könnyen szerkeszthetünk ilyen url-eket a megfelelő csatornákhoz.

A Microsoft csatornájának az RSS Feed URL-e A következő képen néz ki:

 ```html
https://www.youtube.com/feeds/videos.xml?channel_id=UChqrDOwARrxdJF-ykAptc7w
  ```

  Hogy a felhasználói élményt javítsam, az új videókat aszerint töltöm le, ahogy a feliratkozásokat a felhasználó kategóriákba rendezte.

A videók letöltésének a lépései a következők: 

1. Csoportosítom a feliratkozásokat kategóriák szerint, vagy ha nincsenek kategóriák, akkor abc sorrendbe, ötvenesével lesznek csoportosítva. Pontosan úgy ahogy a felhasználói felületen vannak megjelenítve.

2. Indítok egy új feladatot (Task) a TPL segítségével, hogy ne blokkoljam az UI szálat.

3. Egyesével egy foreach ciklussal végig lépkedek a csoportokon.

4. Majd A foreach ciklusban, egy Parallel.Forech ciklussal a csoportokban lévő RSS url-ek segítségével letöltöm az új videókat.

5. Miután letöltöttem a videókat a feliratkozásokhoz tartozó csatornákról, csak azt kell megvizsgálnom, hogy melyek az új videók. Ezt úgy tudom elvégezni, hogy összehasonlítom a letöltött videókat a már korábban letöltött videókkal.