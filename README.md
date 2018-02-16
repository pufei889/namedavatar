# named avatar

Avatar generated by name text based on svg.
Fill `<svg>` as data uri into the `<img>` src, no added element.

如果用户没有个性头像，就填充用户名生成的`<svg>`头像。
程序会在 `<img src>` 上填充data URI，没有额外添加元素。

![demo](https://raw.github.com/joaner/namedavatar/master/demo.png)

## Installtion

```bash
npm install namedavatar --save
```

## Usage

namedavatar is a UMD module, support Browser, Requirejs, Vue2 directive.

**NOTE** that the `<img>` `width` and `border-radius: 100%` requires you additional settings, the program is not set.

### Browser

```html
<img id="avatar1" src="./invalid.jpg" alt="Tom Hanks" width="40" style="border-radius: 100%">

<script src="/dist/namedavatar.min.js"></script>
<script>
namedavatar.config({
  nameType: 'initials'
})

// fill single <img>
(function(img) {
  namedavatar.setImg(img, img.alt)
})(document.getElementById('avatar1'))
</script>
```

if `img.src` invalid, `img#avatar1` will be:
```html
<img id="avatar1" src="data:image/svg+xml,&lt;svg xmlns=&quot;http://www.w3.org/2000/svg&quot; width=&quot;32&quot; height=&quot;32&quot;&gt;&lt;rect fill=&quot;#9C27B0&quot; x=&quot;0&quot; y=&quot;0&quot; width=&quot;100%&quot; height=&quot;100%&quot;&gt;&lt;/rect&gt;&lt;text fill=&quot;#FFF&quot; x=&quot;50%&quot; y=&quot;50%&quot; text-anchor=&quot;middle&quot; alignment-baseline=&quot;central&quot; font-size=&quot;16&quot; font-family=&quot;Verdana, Geneva, sans-serif&quot;&gt;Hanks&lt;/text&gt;&lt;/svg&gt;">
```

### Requirejs

```html
<img data-name="李连杰">
<img src="./invalid.jpg" data-name="Tom Hanks">

<script>
requirejs('namedavatar', function(namedavatar){
  namedavatar.config({
    nameType: 'lastName',
  })
  // set multi <img> use data-name attribute for full name
  namedavatar.setImgs(document.querySelectorAll('img[data-name]'), 'data-name')
})
</script>
```

### Vue2

main.js
```javascript
import { directive } from 'namedavatar/vue'

// register as directive
Vue.directive('avatar', directive);
```

in vue template
```html
<template>
  <img v-avatar="'Tom Hanks'" width="36"/>
</template>
```

## API

### .config(Object options)

| filed    | type   | default | description      |
| -------- | ------ | ------- | ---------------- |
| nameType | string | `'firstName'` | pick from: `firstName`, `lastName`, `initials` |
| fontFamily | string | `'Verdana, Geneva, sans-serif'` | font family |
| backgroundColors | Array | Material Design colors *-500 | random background color list |
| textColor | string | `'#FFF'` | name text color |
| minFontSize | number | `8` | min font size limit |
| maxFontSize | number | `16` | max font size limit |

### .getSVG(string name, Object options)

- `name` the full name value
- `options` extended options

return `<svg>` node, get string by `svg.outerHTML`

### .setImg(HTMLImageElement img, string name)

- `img` `<img>` item
- `name` the full name value

create svg by `name`, and fill to `<img src="data:image/svg+xml,<svg>...">`

### .setImgs(HTMLImageElement[] imgs, string attr)

- `imgs` `<img>` list
- `attr` pick full name value from special attr, eg `'alt'`, `'data-name'`

create svg by `attr` value, batch processing `setImg()`

## Contributing

- IE > 8 (based on [<SVG>](https://caniuse.com/#feat=svg))
- Continuous improvement, welcome review and suggest
