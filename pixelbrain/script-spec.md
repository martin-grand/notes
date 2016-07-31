# Pixelbrain: elérhető változók és metódusok

## goTo ( [string] `pixelId` )
A `goTo` metódus segítségével betölthetsz egy meglévő definiált pixelt. A `pixelId` paraméterrel adjuk meg hogy hova akarunk navigálni. Amint meghívódik ez, átkerül a vezérlés, és az adott script szekció további hívásai érvényét veszítik.

Az alábbi példakódban például adott napszaktól függően más más pixel-re navigáljuk tovább a felhasználót:

```js
var today = new Date(),
    hour = today.getHours();

if (hour < 12) {
    goTo('morning');
} else if (hour < 18) {
    goTo('afternoon');
} else {
    goTo('evening');
}

```

## echo ( [string] `text`, [string, optional] `attachment`, [string, optional] `avatar`)
Az `echo` metódus segítségével kiírathatunk egy buborékot a falhasználónak. A `text` paraméter adja meg hogy milyen szöveget tartalmazzon a buborék, tehát hogy mit modnjon a robotunk a felhasználónak.

Az alábbi példakódban például adott napszaknak megfelelően köszöntjük a felhasználót:

```js
var today = new Date(),
    hour = today.getHours();

if (hour < 12) {
    echo('Good morning!');
} else if (hour < 18) {
    echo('Good afternoon!');
} else {
    echo('Good evening!');
}

```

Van lehetőségünk a szövegbuborékoz az `attachment` paraméterrel mellékletet (linket, képet, videót vagy beáhyazható, lejátszahtó tartalmat) csatolni. Az `attachment` paraméter egy link lehet, és attól függően hogy mire mutat attól függően mutatja a buborék a mellékletet.

Az alábbi példakódban például adott napszaknak megfelelően köszöntjük a felhasználót, és egy hozzátartozó mellékletet is csatolunk a szövegbuborékhoz:

```js
var today = new Date(),
    hour = today.getHours();

if (hour < 12) {
    // link:
    echo('Good morning!', 'https://wikipedia.org/wiki/Good_Morning_America');
} else if (hour < 18) {
    // image:
    echo('Good afternoon!', 'https://website.com/good-afternoon.jpg');
} else {
    // embed:
    echo('Good evening!', 'https://youtu.be/hUF33WAjMas');
}

```

Van lehetőség a szövegbuborékhoz tartozó avatar kép testreszabhatóságára. Alapból a robothoz tarozik egy avatar ami  megjelenik minden szövegbuborékhoz tartozóan, ám ha az `echo` metódus `avatar` paraméterébe egy kép linkjét adjuk meg, ez felülíródik, de csak az aktuális szövegbuborég kiírásának alkalmával.

Az alábbi példakódban háthatjuk hogyan tudunk megjeleníteni csatolmánnyal, vagy csatolmány nélkül testreszabható avatart:

```js
// with attachment:
echo('Good morning!', 'https://wikipedia.org/wiki/Good_Morning_America', 'https://website.com/good-afternoon.jpg');

// without attachment:
echo('Good morning!', false, 'https://website.com/good-afternoon.jpg');

```

## input ( [string] `type`, [object, optional] `options` )
Az input metódussal lehetőség van adato(ka)t bekérni a felhasználótól. Bekérhető sima szöveges tartalom, szám, dátum választás, és még sok féle tartalom. A `type` paraméterrel határozzuk meg a bekérendő adat típusát. 

Az alábbi típusok elérhetőek:
- **text** (sima szöveges bemenet. Az `options` paraméterben 3 paraméter adható meg: `format`, `minimum`, `maximum`) Az alábbi példák mutatják ennek a felhasználását:
    - `input ( 'text' )` egy tetszőleges szöveget kérünk be, ami minimum **1** és maximum **120** karakterből áll. Ez a minimum és maximum az alapértelmezett értéket.
    - `input ( 'text', { minimum : 5, maximum : 160 } )` egy olyan szöveget kérünk be ebben az esetben, ami minimum **5** és maxumum **160** karakterből áll.
    - `input ( 'text', { format : 'email' } )` egy emilcímet kérünk be a felhasználótól.
    - `input ( 'text', { format : 'link' } )` egy linket kérünk be a felhasználótól.
- **number** (egy számot kérhetünk be. Az `options` paraméterben 2 paraméter adható meg: `minimum`, `maximum`) Az alábbi példák mutatják ennek a felhasználását:
    - `input ( 'number' )` egy tetszőleges számot kérünk be.
    - `input ( 'number', { minimum : -100, maximum : 100 } )` egy számot kérünk be **-100** és **100** között.
- **date**
- **select**
- **multiple-select**
- **array**

## data

### data.pixel ( [string] `pixelId` )
Egy adott pixel értékét lehet le kérni. Ha nincs beállítva, vagy nincs ilyen pixel akkor `undefined` választ kapunk.

```js
var name = data.pixel('get_name');

if(!name) {
    goTo('get_name');
}

```

### data.sessionStorage
### data.botStorage
### data.user
#### data.user.get ( [string] `dataName` )
#### data.user.set ( [string] `dataName`, [mixed] `dataValue` )
### data.component
### data.history
### data.dictionary

## hasAccess ( [string] `method` )
## getAccess ( [array] `methods` )

## api
### api.mailToUser ( [string] `userId`, [string] `text`, [date, optional] `date`, [string, optional] `from` )
### api.http ( [string] `method`, [string] `url`, [object, optional] `data`, [object, optional] `header` )
### api.log ( [string] `text`, [string, optional] `label` )
### api.loadComponent ( [string] `componentId`, [string, optional] `scope`, [object, optional] `parameters` )

### api.createPayment ( [string] `from`, [string] `to`, [number] `credit` ) // available later!

### api.createBot ( [object] `source` )
### api.createEvent ( [string] `name`, [mixed, optional] `value` )
Lehetőség van saját eventek küldésére oda ahol az adott robot beágyazásra kerül, így a beágyazó oldal tud reagálni különböző eseményekre.

## importUtility ( [string] `name`, [string, optional] `alias` )
## utility

## delay 
