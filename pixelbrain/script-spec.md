# Pixelbrain: elérhető global változók és metódusok

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

## echo ( [string] `text`, [string, optional] `attachment` )
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

## input ( [object] `settings` )

## data

### data.pixel ( [string] `pixelId` )
Egy adott pixel értékét lehet le kérni. Ha nincs beállítva, vagy nincs ilyen pixel akkor `undefined` választ kapunk.

```js
var name = data.pixel('get_name');
if(!name) {
  goTo('get_name');
}

```

### data.session
### data.user
### data.storage
### data.component

## api
### api.mailTo ( [string] `userId`, [string] `text` )
### api.http ( [string] `method`, [string] `url`, [object, optional] `data`, [object, optional] `header` )

## delay 

## dictionary
