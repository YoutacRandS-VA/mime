```javascript --hide
runmd.onRequire = (path) => path.replace(/^mime/, '../dist/src/index.js');
```

# Mime

[![NPM downloads](https://img.shields.io/npm/dm/mime)](https://www.npmjs.com/package/mime)
[![Mime CI](https://github.com/broofa/mime/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/broofa/mime/actions/workflows/ci.yml?query=branch%3Amain)

An API for MIME type information.

- All `mime-db` types
- Compact and dependency-free [![mime's badge](https://deno.bundlejs.com/?q=mime&badge)](https://bundlejs.com/?q=mime)
- Full TS support


> **Important**
> `mime@4` is currently in **beta**. Changes include:
> * ESM module support is required.  See the [ESM Module FAQ](https://gist.github.com/sindresorhus/a39789f98801d908bbc7ff3ecc99d99c).
> * [ES2020](https://caniuse.com/?search=es2020) support is required
> * Typescript types are built-in
>
> To install:  ` npm install mime@beta`
## Install

```bash
npm install mime
```

## Quick Start

For the full version (800+ MIME types, 1,000+ extensions):

```javascript --run default
import mime from 'mime';

mime.getType('txt'); // RESULT
mime.getExtension('text/plain'); // RESULT
```

### Lite Version [![mime/lite's badge](https://deno.bundlejs.com/?q=mime/lite&badge)](https://bundlejs.com/?q=mime/lite)

`mime/lite` is a drop-in `mime` replacement, stripped of unofficial ("`prs.*`", "`x-*`", "`vnd.*`") types:

```javascript
import mime from 'mime/lite';
```

## API

### `mime.getType(pathOrExtension)`

Get mime type for the given file path or extension. E.g.

```javascript --run default
mime.getType('js'); // RESULT
mime.getType('json'); // RESULT

mime.getType('txt'); // RESULT
mime.getType('dir/text.txt'); // RESULT
mime.getType('dir\\text.txt'); // RESULT
mime.getType('.text.txt'); // RESULT
mime.getType('.txt'); // RESULT
```

`null` is returned in cases where an extension is not detected or recognized

```javascript --run default
mime.getType('foo/txt'); // RESULT
mime.getType('bogus_type'); // RESULT
```

### `mime.getExtension(type)`

Get file extension for the given mime type. Charset options (often included in Content-Type headers) are ignored.

```javascript --run default
mime.getExtension('text/plain'); // RESULT
mime.getExtension('application/json'); // RESULT
mime.getExtension('text/html; charset=utf8'); // RESULT
```

### `mime.getAllExtensions(type)`

> **Note**
> New in `mime@4`

Get all file extensions for the given mime type.

```javascript --run default
mime.getAllExtensions('text/plain'); // RESULT
```

## Custom `Mime` instances

The default objects exported by `mime` are immutable by design.  Mutable versions can be created as follows...
### new Mime(type map [, type map, ...])

Create a new, custom mime instance.  For example, to create a mutable version of the default `mime` instance:

```javascript --run default
import { Mime } from 'mime/lite';

import standardTypes from 'mime/types/standard.js';
import otherTypes from 'mime/types/other.js';

const mime = new Mime(standardTypes, otherTypes);
```

Each argument is passed to the `define()` method, below. For example `new Mime(standardTypes, otherTypes)` is synonomous with `new Mime().define(standardTypes).define(otherTypes)`

### `mime.define(type map [, force = false])`

> **Note**
> Only available on custom `Mime` instances

Define MIME type -> extensions.

Attempting to map a type to an already-defined extension will `throw` unless the `force` argument is set to `true`.

```javascript --run default
mime.define({ 'text/x-abc': ['abc', 'abcd'] });

mime.getType('abcd'); // RESULT
mime.getExtension('text/x-abc'); // RESULT
```

## Command Line

### Extension -> type

    $ mime scripts/jquery.js
    application/javascript

### Type -> extension

    $ npx mime -r image/jpeg
    jpeg
