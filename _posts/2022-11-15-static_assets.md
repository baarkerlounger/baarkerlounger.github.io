---
layout: post
title: Static assets
subtitle: A full stack web app in Rust - Part 4
gh-repo: https://github.com/baarkerlounger/govuk-prototype-rs
tags: [rust, rocket, web dev]
comments: true
---

# Rust
The first thing we need to do is mount a folder that rocket is going to serve static assets from by

```rust
#[launch]
fn rocket() -> _ {
    rocket::build()
      .mount("/assets", FileServer::from(relative!("assets")))
      .mount("/", routes![start_page])
}
```
<br/><br/>
# JS/CSS
Add our [NPM package](https://www.npmjs.com/package/govuk-frontend)
```bash
npm init
npm -i govuk-frontend
```

Then add our entry point files to load and initialise our js/css components. In my case I'm using `assets/styles/application.scss`

```scss
$govuk-assets-path: "govuk-frontend/govuk/assets/";

@import "govuk-frontend/govuk/all.scss";
```
and `assets/scripts/application.js`
```js
import { initAll as GOVUKFrontend } from 'govuk-frontend'

require.context('govuk-frontend/govuk/assets')

GOVUKFrontend()
```

<br/><br/>
# Asset pipeline
After that we mostly just need to do the usual wrangling to transpile, minify, bundle etc our front end assets to the directory we mounted above. This turned out to be more or less the same as in any other web framework and not particularly rocket specific at all. I used [esbuild](https://esbuild.github.io/) to do that and ended up with this config:

package.json
```js
{
  "name": "govuk-prototype-rs",
  "version": "0.1.0",
  "description": "GOVUK Design Project Template",
  "main": "esbuild.js",
  "dependencies": {
    "govuk-frontend": "^4.3.1"
  },
  "devDependencies": {
    "esbuild": "^0.15.12",
    "esbuild-plugin-copy": "^1.6.0",
    "esbuild-sass-plugin": "^2.4.0"
  },
  "scripts": {
    "build": "node esbuild.js"
  },
  "author": "baarkerlounger",
  "license": "MIT"
}
```
esbuild.js
```js
const {sassPlugin, postcssModules} = require('esbuild-sass-plugin')
const {copy} = require('esbuild-plugin-copy')
const path = require('node:path')

require('esbuild')
    .build({
        entryPoints: ['assets/styles/application.scss', 'assets/scripts/application.js'],
        outdir: 'assets/builds',
        bundle: true,
        minify: true,
        loader: {
          '.woff': 'file',
          '.woff2': 'file',
          '.png': 'file',
        },
        plugins: [
          sassPlugin({
            type: 'css',
            includePaths: [
              path.resolve(__dirname, 'node_modules'),
            ]
          }),
          copy({
            assets: {
              from: ['./node_modules/govuk-frontend/govuk/assets/**/*'],
              to: [''],
            },
          }),
      ],
    })
    .then(() => console.log('⚡ Build complete! ⚡'))
    .catch(() => process.exit(1));
```

Last thing left to do is referencing our entrypoint files in our layout template

```HTML
<script src="/assets/builds/scripts/application.js"></script>
<link href="/assets/builds/styles/application.css" rel="stylesheet">
```

and running `node esbuild.js` to build assets ready to serve.
<br/><br/>
This setup doesn't support hot reloading asset changes with file watching yet but that seems like a nice to have and something I can come back to, for the time being this feels workable.
<br/><br/>
