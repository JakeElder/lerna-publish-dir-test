# Lerna publishConfig.dir with Yarn & workspaces error

The `publishConfig.directory` option doesn't work when using `lerna bootstrap --npm-client yarn --use-workspaces`

Running
```
lerna bootstrap
```

without params links the module correctly as

```
packages/app/node_modules/@lerna-publish-dir-test/components ~> packages/components/src
```

```shell
> lerna bootstrap
info cli using local version of lerna
lerna notice cli v3.22.1
lerna info versioning independent
lerna info Bootstrapping 2 packages
lerna info Symlinking packages and binaries
lerna success Bootstrapped 2 packages
> node packages/app/src/app.js
This is a button # Imports the module correctly
```

But then using

```shell
lerna --npm-client yarn --use-workspaces
```

links the root directory of the consumed package (presumably as all the work is delegated to yarn when using these options)

```
node_modules/@lerna-publish-dir-test/components ~> packages/components
```

```shell
> lerna clean
info cli using local version of lerna
lerna notice cli v3.22.1
lerna info versioning independent
lerna info Removing the following directories:
lerna info clean packages/app/node_modules
lerna info clean packages/components/node_modules
? Proceed? Yes
lerna info clean removing /lerna-test/packages/app/node_modules
lerna info clean removing /lerna-test/packages/components/node_modules
lerna success clean finished
> lerna bootstrap --npm-client yarn --use-workspaces
info cli using local version of lerna
lerna notice cli v3.22.1
lerna info versioning independent
lerna info bootstrap root only
yarn install v1.22.4
[1/4] 🔍  Resolving packages...
success Already up-to-date.
✨  Done in 0.38s.
> node packages/app/src/app.js
internal/modules/cjs/loader.js:1023
  throw err;
  ^

Error: Cannot find module '@lerna-publish-dir-test/components/Button'
```
