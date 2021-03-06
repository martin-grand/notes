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

## say ( [string] `content`, [string, optional] `attachment`, [string, optional] `avatar`)
A `say` metódus segítségével kiírathatunk egy buborékot a falhasználónak. A `content` paraméter adja meg hogy milyen szöveget tartalmazzon a buborék, tehát hogy mit modnjon a robotunk a felhasználónak.

Az alábbi példakódban például adott napszaknak megfelelően köszöntjük a felhasználót:

```js
var today = new Date(),
    hour = today.getHours();

if (hour < 12) {
    say('Good morning!');
} else if (hour < 18) {
    say('Good afternoon!');
} else {
    say('Good evening!');
}
```

Van lehetőségünk a szövegbuborékoz az `attachment` paraméterrel mellékletet (linket, képet, videót vagy beáhyazható, lejátszahtó tartalmat) csatolni. Az `attachment` paraméter egy link lehet, és attól függően hogy mire mutat attól függően mutatja a buborék a mellékletet.

Az alábbi példakódban például adott napszaknak megfelelően köszöntjük a felhasználót, és egy hozzátartozó mellékletet is csatolunk a szövegbuborékhoz:

```js
var today = new Date(),
    hour = today.getHours();

if (hour < 12) {
    // link:
    say('Good morning!', 'https://wikipedia.org/wiki/Good_Morning_America');
} else if (hour < 18) {
    // image:
    say('Good afternoon!', 'https://website.com/good-afternoon.jpg');
} else {
    // embed:
    say('Good evening!', 'https://youtu.be/hUF33WAjMas');
}
```

Van lehetőség a szövegbuborékhoz tartozó avatar kép testreszabhatóságára. Alapból a robothoz tarozik egy avatar ami  megjelenik minden szövegbuborékhoz tartozóan, ám ha az `say` metódus `avatar` paraméterébe egy kép linkjét adjuk meg, ez felülíródik, de csak az aktuális szövegbuborég kiírásának alkalmával.

Az alábbi példakódban háthatjuk hogyan tudunk megjeleníteni csatolmánnyal, vagy csatolmány nélkül testreszabható avatart:

```js
// with attachment:
say('Good morning!', 'https://wikipedia.org/wiki/Good_Morning_America', 'https://website.com/good-afternoon.jpg');

// without attachment:
say('Good morning!', false, 'https://website.com/good-afternoon.jpg');
```

## ask ( [string] `type`, [object, optional] `options` )
Az ask metódussal lehetőség van adato(ka)t bekérni a felhasználótól. Bekérhető sima szöveges tartalom, szám, dátum választás, és még sok féle tartalom. A `type` paraméterrel határozzuk meg a bekérendő adat típusát. 

Az ask metódus minden esetben egy `Promise`-t fog vissza adni a bekért érték(ek)kel. A `Promise`-ra egy használati példa eben az esetben, például megkérdezzük a felhasználó nevét, majd köszöntjük őt a saját nevén:
```js
say('What is your name?');

ask('text').then(function(answer){
    say('Hello ' + answer + '!');
});
```

Az alábbi típusok elérhetőek:
- **text** (sima szöveges bemenet. Az `options` paraméterben 3 paraméter adható meg: `format`, `minimum`, `maximum`) Az alábbi példák mutatják ennek a felhasználását:
    - `ask('text')` egy tetszőleges szöveget kérünk be, ami minimum **1** és maximum **120** karakterből áll. Ez a minimum és maximum az alapértelmezett értékek.
    - `ask('text', { minimum : 5, maximum : 160 })` egy olyan szöveget kérünk be ebben az esetben, ami minimum **5** és maxumum **160** karakterből áll.
    - `ask('text', { format : 'email' })` egy emilcímet kérünk be a felhasználótól.
    - `ask('text', { format : 'link' })` egy linket kérünk be a felhasználótól.
- **number** (egy számot kérhetünk be. Az `options` paraméterben 2 paraméter adható meg: `minimum`, `maximum`) Az alábbi példák mutatják ennek a felhasználását:
    - `ask('number')` egy tetszőleges számot kérünk be.
    - `ask('number', { minimum : -100, maximum : 100 })` egy számot kérünk be **-100** és **100** között.
- **time** *not imlemented* (egy időpontot kérhetünk be a felhasználótól. Az `options` paraméterben 2 paraméter adható meg: `minimum`, `maximum`. Itt meghatározhatjuk hogy mi legyen a legkorább és legkésőbbi időpont ami megadható) Az alábbi példák, különböző eshetőségeteket mutatnak be:
    - `ask('time')` így egy pontos időpontot kérhetünk be, mondjuk egy emlékeztető időpontját.
    - `ask('time', { minimum : '9:00', maximum : '17:30' })` így egy időpontot kérhetünk be 9:00 és 17:00 között, például mikorra kér időpontot. A visszatérő érték egy szöveg lesz, pl.: '14:45'
- **date** *not imlemented* (egy dátumot kérhetünk be a felhasználótól. Az `options` paraméterben 2 paraméter adható meg: `minimum`, `maximum`. Itt meghatározhatjuk hogy mi legyen a legkorább és legkésőbbi dátum ami megadható) Az alábbi példák, különböző eshetőségeteket mutatnak be:
    - `ask('date')` így egy tetszőleges dátumot kérhetünk be.
    - `ask('date', { minimum : new Date(1900,0,14), maximum : new Date() })` így egy időpontot kérhetünk be 1900 Január 14. és a mai dátum között.  A visszatérő érték egy dátum objektum lesz.
- **dateTime** *not imlemented* (egy dátumidőt kérhetünk be a felhasználótól. Az `options` paraméterben 4 paraméter adható meg: `minimumDate`, `maximumDate`, `minimumTime`, `maximumTime`,. Itt meghatározhatjuk hogy mi legyen a legkorább és legkésőbbi dátum ami megadható, illetve a legkorábbi és legkésőbbi időpont) Az alábbi példák, különböző eshetőségeteket mutatnak be:
    - `ask('dateTime')` így egy tetszőleges dátumidőt kérhetünk be.
    - `ask('dateTime', { minimumDate : new Date(1900,0,14), maximumDate : new Date(), minimumTime : '9:00', maximumTime : '17:30' })` így egy időpontot kérhetünk be 1900 Január 14. és a mai dátum között 9:00 és 17:00 közti időpontban.  A visszatérő érték egy dátum objektum lesz.
- **select** (álltalunk meghatározott értékek közül kínálhatunk fel választási lehetőséget a felhasználónak) Az `options` paraméterben egy values tömböt vár az ask, ahol megadhatjuk hogy mi kerüljün kiírásra és milyen értéken, az alábbi módon:
```js
say('Which are not fruit?');

ask('select', { value : [
    { label : 'apple', value : 0 },
    { label : 'pear', value : 0 },
    { label : 'carrot', value : 1 }
]}).then(function(answer){
    if(answer.value === 0) {
        goTo('wrong-answer');
    } else {
        goTo('good-answer');
    }
});
```
- **multiple-select** (álltalunk meghatározott értékek közül kínálhatunk fel több választási lehetőséget a felhasználónak) az érték feltöltés ugyan úgy zajlik, mint a **select** típusnál, az `options` kiegészül egy `minimum` és egy `maxumum` paraméterrel, amik azt határozzák meg hogy minimum és maximum mennyi értéket választhat ki a felhasnáló. Alapból a minimum értéke **0** a maximumé pedig korlátlan, tehát az összes opció kiválasztható:
```js
say('Which are fruit(s)?');

ask('multiple-select', { minimum : 1, maximum : 2, value : [
    { label : 'apple', value : 0 },
    { label : 'pear', value : 0 },
    { label : 'carrot', value : 1 }
]}).then(function(answer) {
    var correctAnswer = 0,
        i;
    
    for(i = 0; i < answer.length; i++) {
        if(answer[i].value === 0) {
            correctAnswer++;
        }
    }
    if(correctAnswer === 2) {
        goTo('good-answer');
    } else {
        goTo('wrong-answer');
    }
});
```
- **array** A felhasználónak lehetősége van tömböt / listát megadnia, Az `options` paraméterben azt határozhatjuk meg, hogy minimum hány elemű tömböt várunk el, és maxumum hány elemet adhat meg. Mindez a `minimum` és `maximum` paraméterek segítségével. Alapértelmezetten minimum **1**, maximum pedig **10** elem adható meg. Például:
    - `ask('array', { minimum : 2, maximum : 20 })` egy olyan tömböt ad vissza, amit a felhasználó meg ad és legfeljebb 20, legalább 2 elemből fog állni.

## data
A `data` különböző adatrétegek elérésére kínál számunkra lehetőséget. Lássuk sorban mire is.

### data.pixel ( [string] `pixelId` )
Egy adott pixel értékét lehet le kérni. Ha nincs beállítva, vagy nincs ilyen pixel akkor `undefined` választ kapunk.

```js
var name = data.pixel('get_name');

if(!name) {
    goTo('get_name');
}

```

### data.sessionStorage
Egy adot beszélgetéshez tartozó adatstruktúrát tárolhatunk a `data.sessionStorage`-ban. Két metódusa van. A `set` és a `get`. Lássuk a használati példákat:
#### data.sessionStorage.get ( [string] `name` )
Ezzel a `data.sessionStorage`-ból olvashatunk. A `name`-hez tartozó értéket adja vissza. Amennyiben nem tartozik hozzá érték `undefined` értéket kapunk. Lássunk egy gyakorlati példát, ahol megvizsgáljuk hogy a `data.sessionStorage` tartalmaz e **name** néven értéket, és ha igen köszöntsük a felahasználót a már korábban tárolt nevén:

```js
if(data.sessionStorage.get('name')) {
    say('Hello ' + name + '!');
} else {
    say('Hello My Friend!');
}
```

#### data.sessionStorage.set ( [string] `name`, [mixed] `value` )
Ezzel a `data.sessionStorage`-ba írhatunk. A `name` határozza meg hogy milyen kulcs néven írunk, a `value` pedig hogy milyen értéket. Lássunk egy gyakorlati példát, ahol megvizsgáljuk hogy a `data.sessionStorage` tartalmaz e **name** néven értéket, és ha nem kérjük be azt és írjuk be a `data.sessionStorage`-ba:

```js
data.sessionStorage.get('name').then(function(name){
    if(name) {
        say('Hello ' + name + '!');
    } else {
        say('What is your name?');
        ask('text').then(function(name){
            data.sessionStorage.set('name', name);
            say('Hello ' + name + '!');
        });
    }
});
```

### data.botStorage
### data.user
#### data.user.get ( [string] `name` )
ahol a `name` lehet 'name', 'email', 'settings', 'attributes'
#### data.user.set ( [string] `name`, [string] `value`, [boolean] `public` )
#### data.user.setting ( [string] `name`, [string] `value` )
### data.component
### data.history
### data.dictionary

## hasAccess ( [string] `method` )
Lehetőség van jogosultságok vizsgálatára. A `method` paraméter egy adott művelet nevét várja. Meg kérdezhetjük hogy van e jogosultságunk erre műveletre a felhasználóval kapcsolatban. Így egy `Promise`-t kapunk, aminek egy paramétere van, ami tartalmazza, hogy van e vagy nincs jogosultságunk. Például megtudhatjuk e a nevét ha akarjuk:

```js
hasAccess('user.name').then(function(hasAccess){
    if(hasAccess) {
        say('I have acces to read your name! :)');
    } else {
        say('I have no acces to read your name! :(');
    }
});
```

## getAccess ( [array] `methods` )
Lehetőség van megkérni a felhasználót hogy adjon jogosultságot műveletekhez ha nem lenne. A `method` paraméter egy tömböt vár, ami tartalmazza a jogosultság kulcsokat. Visszatérési értékként egy `Promise`-t kapunk, aminek egy paramétere van, ami tartalmazza, hogy a felhasználó engedélyezte vagy sem a jogosultságokat.

```js
getAccess(['user.name', 'user.email']).then(function(hasAccess){
    if(hasAccess) {
        say('Thank you for add acces to read this things!');
    } else {
        say('I\'m sorry to reject my request..:(');
    }
});
```
A következő accessek vannak:
- `user.name` a felhasználó nevének olvasása.
- `user.email` a felhasználó emailcímének olvasása.
- `user.settings` a felhasználó beállításainak olvasása.
- `user.attributes` a felhasználóhoz társított információk olvasása, amit más robotok vagy mi írtunk bele.
- `user.writePublic` a felhasználóhoz társíthatunk információkat, amiket más robotok is láthatnak ha kapnak `user.attributes` jogosultságot.
- `user.writePrivate` a felhasználóhoz társíthatunk információkat, amiket csak mi láthatunk, ha kapunk `user.attributes` jogosultságot.
- `user.writeSettings` a felhasználó beállításainak írás joga.

## api
### api.mailToUser ( [string] `userId`, [string] `text`, [date, optional] `date`, [string, optional] `from` )
### api.http ( [string] `method`, [string] `url`, [object, optional] `data`, [object, optional] `header` )
### api.log ( [string] `text`, [string, optional] `label` )
### api.loadComponent *not imlemented* ( [string] `componentId`, [string, optional] `scope`, [object, optional] `parameters` )

### api.createPayment *not imlemented* ( [string] `from`, [string] `to`, [number] `credit` )
### api.applyPayment *not imlemented* ( [string] `id` )
### api.cancelPayment *not imlemented* ( [string] `id` ) 

### api.createBot *not imlemented* ( [object] `source` )
### api.createArticle *not imlemented* ( [object] `source` )

### api.createEvent ( [string] `name`, [mixed, optional] `value` )
Lehetőség van saját eventek küldésére oda ahol az adott robot beágyazásra kerül, így a beágyazó oldal tud reagálni különböző eseményekre.

### api.open ( [string] `url`, [string, optional] `method`, [object, optional] `data` )
Ennek a metódusnak a meghívásával elnavigálhatjuk a felhasználót egy webcímre.

## importUtility *not imlemented* ( [string] `name`, [string, optional] `alias` )
## utility

## delay 
