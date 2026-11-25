## Overview :

### Bypassing flawed key sanitization -

An obvious way in which websites attempt to prevent prototype pollution is by sanitizing property keys before merging them into an existing object. However, a common mistake is failing to recursively sanitize the input string. For example, consider the following URL:
```js
vulnerable-website.com/?__pro__proto__to__.gadget=payload
```

If the sanitization process just strips the string __proto__ without repeating this process more than once, this would result in the following URL, which is a potentially valid prototype pollution source:

```js
vulnerable-website.com/?__proto__.gadget=payload
```

## Lab :

<img width="892" height="387" alt="image" src="https://github.com/user-attachments/assets/bef4a6e2-3e61-4360-bfcf-e11c5d0b8dcb" />

## Solution

### Identifying Source -

<img width="1919" height="568" alt="image" src="https://github.com/user-attachments/assets/90813673-f04d-4ba8-8cca-bcf57ac59e18" />

Using Dot notation does not pollute the object protoype

<img width="946" height="399" alt="image" src="https://github.com/user-attachments/assets/f265cf08-705d-49f6-81b1-e4b84112f4d7" />

Using Bracket notation a well does not pollute the object prototype

<img width="1541" height="403" alt="image" src="https://github.com/user-attachments/assets/f259acdb-26b2-4603-a999-48d625b20ddf" />

Thats because the site loads `deparamSanitised.js` script to sanitize the user input prototype payload. And that script uses the `sanitizeKey()` function defined in searchLoggerFiltered.js to strip potentially dangerous property keys based on a blocklist. However, it does not apply this filter recursively.

```js
//Sanitizekey() in Searchlogger.js

function sanitizeKey(key) {
    let badProperties = ['constructor','__proto__','prototype'];
    for(let badProperty of badProperties) {
        key = key.replaceAll(badProperty, '');
    }
    return key;
}
```
The below is the **deparamSanitised.js** content.
```js
var deparam = function( params, coerce ) {
    var obj = {},
        coerce_types = { 'true': !0, 'false': !1, 'null': null };

    if (!params) {
        return obj;
    }

    params.replace(/\+/g, ' ').split('&').forEach(function(v){
        var param = v.split( '=' ),
            key = decodeURIComponent( param[0] ),
            val,
            cur = obj,
            i = 0,

            keys = key.split( '][' ),
            keys_last = keys.length - 1;

        if ( /\[/.test( keys[0] ) && /\]$/.test( keys[ keys_last ] ) ) {
            keys[ keys_last ] = keys[ keys_last ].replace( /\]$/, '' );

            keys = keys.shift().split('[').concat( keys );

            keys_last = keys.length - 1;
        } else {
            keys_last = 0;
        }

        if ( param.length === 2 ) {
            val = decodeURIComponent( param[1] );

            if ( coerce ) {
                val = val && !isNaN(val) && ((+val + '') === val) ? +val        // number
                    : val === 'undefined'                       ? undefined         // undefined
                        : coerce_types[val] !== undefined           ? coerce_types[val] // true, false, null
                            : val;                                                          // string
            }

            if ( keys_last ) {
                for ( ; i <= keys_last; i++ ) {
                    key = keys[i] === '' ? cur.length : keys[i];
                    cur = cur[sanitizeKey(key)] = i < keys_last
                        ? cur[sanitizeKey(key)] || ( keys[i+1] && isNaN( keys[i+1] ) ? {} : [] )
                        : val;
                }

            } else {
                if ( Object.prototype.toString.call( obj[key] ) === '[object Array]' ) {
                    obj[sanitizeKey(key)].push( val );

                } else if ( {}.hasOwnProperty.call(obj, key) ) {
                    obj[sanitizeKey(key)] = [ obj[key], val ];

                } else {
                    obj[sanitizeKey(key)] = val;
                }
            }

        } else if ( key ) {
            obj[key] = coerce
                ? undefined
                : '';
        }
    });

    return obj;
};
```

So to bypass this, our payload should contain entries or have nested entries that bypasses the blacklisted filter.

```js
    let badProperties = ['constructor','__proto__','prototype'];
```

### Identifying gadget -

<img width="1908" height="496" alt="image" src="https://github.com/user-attachments/assets/76009928-6b17-4034-a8e2-50e76ae775fd" />

DOM Invader identifies that `transport_url` is the gadget which is not defined.

<img width="623" height="457" alt="image" src="https://github.com/user-attachments/assets/c03c1db5-3849-4cc7-856e-0ae691bc3a79" />

Notice that `transport_url` is a property set to `config` object & it is not explicitly defined. Each time it takes the value from the URL parameters. We can pollute this through the URI

###  Exploit -

Upon using the nested payload, we can trigger the XSS.
<img width="1575" height="268" alt="image" src="https://github.com/user-attachments/assets/7d96c82b-e23b-44df-8554-8c2dfb9fce3a" />
