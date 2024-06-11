# nvim-busted-action

A composite [GitHub action](https://github.com/features/actions)
for running [busted](https://lunarmodules.github.io/busted/) tests
with [Neovim](https://github.com/neovim/neovim).

Supports stable releases, nightly releases and specifying versions.

## Usage

### Setup

To run tests with `busted`, you need

- A [rockspec](./example-scm-1.rockspec), to specify test dependencies.
  It should have `nlua` in its `test_dependencies` list.
  See also: [Rockspec format](https://github.com/luarocks/luarocks/wiki/Rockspec-format)
- A [`.busted` file](./.busted) that tells `busted` how to run your tests
  with Neovim as the Lua interpreter.

### Example

```yaml
---
name: Run tests
on:
  pull_request: ~
  push:
    branches:
      - main

jobs:
  build:
    name: Run tests
    runs-on: ubuntu-latest
    strategy:
      matrix:
        neovim_version: ['nightly', 'stable']

    steps:
      - uses: actions/checkout@v4
      - name: Run tests
        uses: nvim-neorocks/nvim-busted-action@v1
        with:
          nvim_version: ${{ matrix.neovim_version }}
```

### Running tests locally

With the above [setup](#setup), you can run tests locally,
if you have [`luarocks`](https://luarocks.org/) or `busted` installed[^1].

[^1]: The test suite assumes that `nlua` has been installed
      using luarocks into `~/.luarocks/bin/`.

You can then run:

```bash
luarocks test --local
# or
busted
```

Or if you want to run a single test file:

```bash
luarocks test spec/path_to_file.lua --local
# or
busted spec/path_to_file.lua
```

## Inputs

### `nvim_version`

Version of Neovim to install. Valid values are `stable`, `nightly` or version tag such
as `v0.9.2`. 

- Default: `stable`

> [!IMPORTANT]
> 
> This value must exactly match to a tag name 
> when installing the specific version.

### `luarocks_version`

Version of LuaRocks[^2] to install.

- Default: `3.11.1`

[^2]: This action uses [LuaRocks](https://luarocks.org/)
      to install `busted` and test dependencies.

## Resources

- [`nvim-lua/nvim-lua-plugin-template`](https://github.com/nvim-lua/nvim-lua-plugin-template/)
- [Using Neovim as Lua interpreter with Luarocks](https://zignar.net/2023/01/21/using-luarocks-as-lua-interpreter-with-luarocks/)
- [Testing Neovim plugins with Busted](https://hiphish.github.io/blog/2024/01/29/testing-neovim-plugins-with-busted/)
- [Test your Neovim plugins with luarocks & busted](https://mrcjkb.dev/posts/2023-06-06-luarocks-test.html)
- [Debugging Lua in Neovim](https://zignar.net/2023/06/10/debugging-lua-in-neovim/)
