## Quick start

### Install

```shell
$ npm install cube-ui --save
```

### Usage

It is recommended to use [babel-plugin-transform-modules](https://www.npmjs.com/package/babel-plugin-transform-modules),which helps you import component modules and corresponding styles more elegantly.

Before use, the plugin needs some configuration. Modify .babelrc:

```json
{
  "plugins": ["transform-modules", {
    "cube-ui": {
      "transform": "cube-ui/lib/${member}",
      "kebabCase": true,
      "style": true
    }
  }]
}
```

If not using babel-plugin-transform-modules, you need to import corresponding style files by hand:

```js
import 'cube-ui/lib/style.css'
```

**Notice:** By default cube-ui will use [post-compile](#/en-US/docs/post-compile) with webpack 2+, but post-complie needs some dependencies and configuration. If you don't want to use post-compile, just modify the webpack config file:

```js
// webpack.config.js

module.exports = {
  // ...
  resolve: {
    // ...
    // https://webpack.js.org/configuration/resolve/#resolve-mainfields
    mainFields: ["main"]
  }
  // ...
}
```

#### Fully import

Commonly in the entry file:

```javascript
import Vue from 'vue'
import Cube from 'cube-ui'

Vue.use(Cube)
```

#### Import on demand

```javascript
import { Button } from 'cube-ui'
```

You can choose to register globally or partially:

```js
// register globally
Vue.use(Button)

// or register partially
// in certain somponents
{
  components: {
    CubeButton: Button
  }
}
```
All the components that can be imported on demand are listed below:

```js
import {
  Button,
  Checkbox,
  Loading,
  Tip,
  Toast,
  Picker,
  TimePicker,
  Dialog,
  ActionSheet,
  Scroll,
  Slide,
  IndexList
} from 'cube-ui'
```

#### Examples

```html
<template>
  <cube-button @click="showDiaog">show dialog<cube-button>
</template>

<script>
  export default {
    methods: {
      showDialog() {
        this.$createDialog({
          type: 'alert',
          title: 'Alert',
          content: 'dialog content'
        }).show()
      }
    }
  }
</script>
```


### Use post-compile

Since cube-ui will use [post-compile](#/en-US/docs/post-compile) with webpack 2+ by default, your application's webpack and babel configuration needs to be compatible with cube-ui.

Follow the steps below:

1. Modify package.json

  ```json
  {
    // webpack-post-compile-plugin depends on compileDependencies
    "compileDependencies": ["cube-ui"],
    "devDependencies": {
      // add stylus dependencies
      "stylus": "^0.54.5",
      "stylus-loader": "^2.1.1",
      "webpack-post-compile-plugin": "^0.1.2"
    }
  }
  ```

2. Modify .babelrc：

  ```json
  {
    "plugins": ["transform-modules", {
      "cube-ui": {
        "transform": "cube-ui/src/modules/${member}",
        "kebabCase": true
      }
    }]
  }
  ```

3. Modify webpack.base.conf.js

  ```js
  var PostCompilePlugin = require('webpack-post-compile-plugin')
  module.exports = {
    // ...
    plugins: [
      // ...
      new PostCompilePlugin()
    ]
    // ...
  }
  ```

4. Modify `exports.cssLoaders` function in build/utils.js

  ```js
  exports.cssLoaders = function (options) {
    // ...
    const stylusOptions = {
      'resolve url': true
    }
    // https://vue-loader.vuejs.org/en/configurations/extract-css.html
    return {
      css: generateLoaders(),
      postcss: generateLoaders(),
      less: generateLoaders('less'),
      sass: generateLoaders('sass', { indentedSyntax: true }),
      scss: generateLoaders('sass'),
      stylus: generateLoaders('stylus', stylusOptions),
      styl: generateLoaders('stylus', stylusOptions)
    }
  }
  ```

  See [https://github.com/vuejs-templates/webpack/pull/970/files](https://github.com/vuejs-templates/webpack/pull/970/files)
