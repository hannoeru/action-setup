# Setup pnpm

Install pnpm package manager.

## Inputs

### `version`

Version of pnpm to install.

**Optional** when there is a [`packageManager` field in the `package.json`](https://nodejs.org/api/corepack.html).

otherwise, this field is **required** It supports npm versioning scheme, it could be an exact version (such as `6.24.1`), or a version range (such as `6`, `6.x.x`, `6.24.x`, `^6.24.1`, `*`, etc.), or `latest`.

### `dest`

**Optional** Where to store pnpm files.

### `run_install`

**Optional** (_default:_ `null`) If specified, run `pnpm install`.

If `run_install` is either `null` or `false`, pnpm will not install any npm package.

If `run_install` is `true`, pnpm will install dependencies recursively.

If `run_install` is a YAML string representation of either an object or an array, pnpm will execute every install commands.

#### `run_install.recursive`

**Optional** (_type:_ `boolean`, _default:_ `false`) Whether to use `pnpm recursive install`.

#### `run_install.cwd`

**Optional** (_type:_ `string`) Working directory when run `pnpm [recursive] install`.

#### `run_install.args`

**Optional** (_type:_ `string[]`) Additional arguments after `pnpm [recursive] install`, e.g. `[--frozen-lockfile, --strict-peer-dependencies]`.

## Outputs

### `dest`

Expanded path of inputs#dest.

### `bin_dest`

Location of `pnpm` and `pnpx` command.

## Usage example

### Just install pnpm

```yaml
name: CI

on:
  push:
    branches:
      - main

jobs:
  install:
    runs-on: ubuntu-latest
    steps:
      - name: Install pnpm
        uses: pnpm/action-setup@v2
        with:
          version: 7
```

### Install pnpm with Node.js and a few npm packages

```yaml
steps:
  - name: Checkout Repo
    uses: actions/checkout@v3

  - name: Install Node.js
    uses: actions/setup-node@v3
    with:
      node-version: 16

  - name: Install pnpm
    uses: pnpm/action-setup@v2
    with:
      version: 7
      run_install: |
        - recursive: true
          args: [--frozen-lockfile, --strict-peer-dependencies]
        - args: [--global, gulp, prettier, typescript]
```

### Use cache to reduce installation time

```yaml
steps:
  - name: Checkout Repo
    uses: actions/checkout@v3

  - name: Install Node.js
    uses: actions/setup-node@v3
    with:
      node-version: 16
      cache: pnpm

  - name: Install pnpm
    uses: pnpm/action-setup@v2
    with:
      version: 7

  - name: Install Dependencies
    run: pnpm install
```

**Note:** You don't need to run `pnpm store prune` at the end; post-action has already taken care of that.

## Notes

This action does not setup Node.js for you, use [actions/setup-node](https://github.com/actions/setup-node) yourself.

## License

[MIT](https://git.io/JfclH) © [Hoàng Văn Khải](https://github.com/KSXGitHub/)
