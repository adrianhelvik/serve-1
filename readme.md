# Proposal

Non-url imports could be prefixed with e.g. `/ESM/` to create a url.
This would allow module resolution to be performed server side. For
imports from an ESM module this would have to be 

## Proposed usage

```javascript
import React from 'react' // Resolves to the url "/ESM/react"
```

## (An incomplete list of) working modules

For now, React does not work with this module, there are however
several modules that I've found work fine.

- preact (through the module-field)
- left-pad (through module.exports compat mode)
- mobx (through the module-field)

## Non-working modules
- React (uses require within the main file)
- mobx-react (imports other modules. Would work with this proposal)

# serve-esm

This is an experimental module for integrating the ESM modules with node_modules. It is not complete,
as that would require transpilation. It is intended as a proposal for how module loading without
a prefix could be resolved server-side instead of increasing complexity on the frontend.
A simple prefix `/ESM/` is added before modules that you wish to import

## A note on this fork

This is a fork of the `serve` project. It adds support for node_modules, with some important caveats.
A module must be imported like this:

```javascript
// Import preact by using the module field in package.json
import { h, Component } from '/ESM/preact'
```

There is also support for module.exports.

```javascript
// Import left-pad by using the main field in package.json
// and exporting module.exports
import leftPad from '/ESM/left-pad'
```
There is however no support for transitive imports or requires.

```javascript
// This will fail because of transitive imports
import mobxReact from '/ESM/mobx-react'
```

## Usage

Install it:

```bash
npm install -g serve-esm
```

And run this command in your terminal:

```bash
serve-esm [options] <path>
```

### More info

See [The original repo](https://github.com/zeit/serve) for more usage information.
