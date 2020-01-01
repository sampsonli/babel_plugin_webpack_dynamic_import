# babel-plugin-webpack-dynamic-import

Babel plugin to transpile `import()` to a deferred `require.ensure()`. and add below code, you can control your module hot reload more flexible
~~~javascript
require.ensure([], (require) => {
     const result = require(SOURCE);
     resolve(result);
     if(module.hot) {
        Promise.resolve().then(() => {
            typeof result.onUpdate === 'function' && module.hot.accept(SOURCE, () => {
                result.onUpdate(require(SOURCE));
            });
        })
     }
}, MODEL);
~~~
you can declare onUpdate method on your module when changing

**NOTE:** Babylon >= v6.12.0 is required to correctly parse dynamic imports.

## Installation

```sh
npm install babel-plugin-webpack-dynamic-import --save-dev
```

## Usage

### Via `.babelrc` (Recommended)

**.babelrc**

```json
{
  "plugins": ["webpack-dynamic-import"]
}
```

#### Options
```json
{
  "plugins": "webpack-dynamic-import"
}
```

### Via CLI

```sh
$ babel --plugins dynamic-import-node script.js
```

### Via Node API

```javascript
require('babel-core').transform('code', {
  plugins: ['webpack-dynamic-import']
});
```
