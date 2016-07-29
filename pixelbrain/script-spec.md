# Pixelbrain: elérhető global változók és metódusok

## goTo ( [string] pixelId )
A goTo metódus segítségével betölthetsz egy meglévő definiált pixelt. Amint meghívódik ez, átkerül a vezérlés, és az adott script szekció további hívásai érvényét veszítik.

## echo ( [string] text, [object, optional] attachment )

## input ( [object] settings )

## data

### data.pixel ( [string] pixelId )
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
### api.mailTo ( [string] userId, [string] text )
### api.http ( [string] method, [string] url, [object, optional] data, [object, optional] header )

## delay 

## dictionary
