This is a driver for all your [pure-most Cycle apps](https://github.com/cyclejs/most-run) (think[Motorcycle](https://github.com/motorcyclejs/core#merging-with-cyclejs)) that needs a router only for the client.

It aims to be compatible with [cyclic-router](https://github.com/cyclejs-community/cyclic-router). The problem with this is that, besides being too bloated (with all the history package inside and plumbing), it focuses too much on server-side routing, so some glitches appears unexpectedly when you use the hash-router. Hash routers are arguably simpler, so I guess you can use this package if you don't need server-side routing.

### Install

```
npm install --save cycle-hashrouter-most-driver
```


### Use

```javascript
import most from 'most'
import hold from '@most/hold'
import Cycle from '@cycle/most-run'
import {makeDOMDriver, h} from '@motorcycle/dom'
import hashRouterDriver from 'cycle-hashrouter-most-driver'

Cycle.run(app, {
  DOM: makeDOMDriver('#container', [
    require('snabbdom/modules/props'),
    require('snabbdom/modules/style')
  ]),
  ROUTER: hashRouterDriver
})

function app ({DOM, ROUTER}) {
  let match$ = hold(
    ROUTER.define({
      '/': {where: 'HOME'},
      '/item/:itemId': id => ({where: 'ITEM', id})
    })
  )

  let vtree$ = match$
    .map(({path, qs, value}) =>
      h('div',
        `you are on path ${path}, querystring ${qs},
        matching the path of ${value.where} and maybe the id ${value.id}.`
      )
    )

  return {
    DOM: vtree$,
    ROUTER: most.of('/item/2143').delay(2000)
  }
}
```
