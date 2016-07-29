# Pixelbrain: elérhető global változók és metódusok

## goTo ( pixelId )
A goTo metódus segítségével betölthetsz egy meglévő definiált pixelt. Amint meghívódik ez, átkerül a vezérlés, és az adott script szekció további hívásai érvényét veszítik.

## echo ( text, (optional) attachment )

## input ( settings )

## data

### data.pixel (pixelId)
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
### api.mailTo ( userId, text )
### api.http ( method, url, data, header )

## delay 

## dictionary
