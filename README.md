# Befektetési napló

Saját, önállóan tesztelt és iteratívan finomított projekt: egy böngészőben futó befektetési portfólió-nyomkövető, amelyet AI ügynökkel (Claude) építettem, miközben magam töltöttem be a tesztelő és termékgazda szerepét.

**Élő demó:** _ide kerül a GitHub Pages link_

## Miről szól ez a projekt?

Nem egy klasszikus "AI-jal írattam egy appot" projekt — a hangsúly a **tesztelési és finomítási folyamaton** van. Minden funkciónál végigmentem azon, hogy: kipróbáltam, hibát vagy zavaró UX-részletet találtam, pontosan megfogalmaztam a problémát és a hatását, majd az AI ügynökkel együttműködve kijavítottuk. Ez a munkamódszer közvetlenül lefedi, amit egy AI-agent tesztelési / UX-finomítási szerepkör igényel.

## Funkciók

- Portfólió-tételek felvétele, törlése, típus szerinti bontással (részvény, kötvény, kripto, készpénz)
- Automatikus kockázati besorolás típusonként, súlyozott portfólió-szintű kockázati mutatóval és koncentrációs figyelmeztetéssel
- Összesített "portfólió-egészség" pontszám (diverzifikáció + kockázat + hozam) narratív magyarázattal
- „Mi lenne, ha” piaci szimulátor: feltételezett piaci elmozdulás típusonként eltérő érzékenységgel
- Élő piaci referencia-grafikon (VOO / S&P 500 ETF, Alpha Vantage API), összevetve a portfólió jelenlegi összhozamával
- Saját, helyi (böngészőben tárolt) historikus nyomkövetés, ami minden megnyitáskor valódi adatponttal bővül
- Excel export valódi cellaformázással (szín, keret, számformátum)
- Rendezhető és szűrhető táblázat
- Mentés/betöltés fájlba (JSON export-import), és automatikus mentés a böngészőben
- Sötét/világos mód
- Lépcsőzetes belépő-animáció

## Technológia

Egyetlen önálló, függőségmentesen megnyitható HTML fájl. Külső könyvtárak CDN-ről: [Chart.js](https://www.chartjs.org/) (diagramok), [xlsx-js-style](https://github.com/gitbrent/xlsx-js-style) (formázott Excel export). Élő piaci adat: [Alpha Vantage](https://www.alphavantage.co/) API.

## Beüzemelés

1. Töltsd le az `index.html` fájlt, vagy nyisd meg közvetlenül a GitHub Pages linken.
2. Az élő piaci grafikonhoz szükséged lesz egy ingyenes Alpha Vantage API-kulcsra ([itt igényelhető](https://www.alphavantage.co/support/#api-key), kb. 30 másodperc). Az oldal első megnyitásakor bekéri, és elmenti a böngésződben.
3. Minden más adat (a felvett eszközeid, a historikus nyomkövetés) is helyben, a böngésződben tárolódik — nem küldünk semmit szerverre.

## Fejlesztési napló (kiemelt tanulságok)

A teljes, részletes teszt-riport (probléma → hatás → javítás struktúrában) külön dokumentumban érhető el. Néhány kiemelt tanulság:

- **Színkód-ütközés:** a kategória-színek és a teljesítmény-színek (piros/zöld) eredetileg ugyanazt a paletta-elemet használták két különböző jelentéssel — ez félrevezető volt, külön palettára cseréltem.
- **Race condition az animációban:** gyors egymás utáni módosításnál két animációs ciklus futott egyszerre ugyanazon az elemen, ami vizuális villogást okozott. Javítás: az animáció a ténylegesen megjelenített értékből folytatódik, nem egy rögzített célértékből.
- **Hibavédelem hiánya:** ha egy külső diagram-könyvtár nem töltődött be, az egész felület renderelése megszakadt utána — beleértve olyan, látszólag független funkciókat is, mint az API-kulcs beviteli mező megjelenítése. Ez kétszer is előfordult (két különböző helyen), és mindkétszer defenzív hibakezeléssel (try/catch) javítottam, hogy egy alkotóelem hibája ne rántsa magával a többit.
- **Külső adatforrások megbízhatatlansága:** két egymást követő "ingyenes, kulcs nélküli" piaci adatforrás (Stooq, majd Yahoo Finance) idővel bot-elleni védelmet vagy hitelesítést vezetett be, ami használhatatlanná tette őket szkript alapú lekérésre. Végső megoldás: hivatalos, kulcsos API (Alpha Vantage).
- **Adathitelesség:** tudatosan nem építettem be kitalált historikus portfólió-adatot, még akkor sem, amikor ez látványosabb lett volna — helyette valódi, helyben gyűjtött napi pillanatképekre és egy egyértelműen megjelölt, élő piaci referenciára támaszkodik az összehasonlítás.

## Szerző

Tóth György Milán — 2026
