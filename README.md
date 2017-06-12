# chunk-manifest-webpack-plugin-js

## (forked from [soundcloud/chunk-manifest-webpack-plugin](https://github.com/soundcloud/chunk-manifest-webpack-plugin) until they are accepting similar PRs)

Allows exporting a JSON file that maps chunk ids to their resulting asset files. Webpack can then read this mapping, assuming it is provided somehow on the client, instead of storing a mapping (with chunk asset hashes) in the bootstrap script, which allows to actually leverage long-term caching.

## Usage

Install via npm:

```shell
npm install --save-dev chunk-manifest-webpack-plugin-js
```

Install via yarn:

```shell
yarn add --dev chunk-manifest-webpack-plugin-js
```

And then require and provide to webpack:

```javascript
// in webpack.config.js or similar
const ChunkManifestPlugin = require('chunk-manifest-webpack-plugin-js');

module.exports = {
  // your config values here
  plugins: [
    new ChunkManifestPlugin({
      filename: 'manifest',
      manifestVariable: 'webpackManifest',
      inlineManifest: false
    })
  ]
};
```

### Options

#### `filename`

Where the manifest will be exported to on bundle compilation. This will be relative to the main webpack output directory. Default = `"manifest.json"`

#### `manifestVariable`

What JS variable on the client webpack should refer to when requiring chunks. Default = `"webpackManifest"`

#### `inlineManifest`

Whether or not to write the manifest output into the [html-webpack-plugin](https://github.com/ampedandwired/html-webpack-plugin). Default = `false`

#### `outputType`
Can be either `js` or `json` (default). If `json`, the files are extracted into a json file; if `js`, the files JSON is parsed and added to a standalone javascript file, where the object is set equal to `window[manifestVariable]`. For cases where inline javascript should be avoided, use `js`.

```jade
// outputType: 'json'
<head>
    window.webpackManifest = JSON.parse(#{manifest.json})
</head>
```

```jade
// outputType: 'js'
<head>
    <script src='webpackManifest.js'></script>
</head>
```
