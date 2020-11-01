# storybook-yarn-berry-monorepo

Repro case for
https://github.com/storybookjs/storybook/issues/9527#issuecomment-720045441

## Steps

- Clone repo

- Install dependencies

`yarn install`

- Start storybook

`yarn workspace my-lib storybook`

## 1st error: core-js

```
ERROR in ./stories/Button.tsx
Module not found: Error: Your application tried to access core-js, but it isn't declared in your dependencies; this makes the require call ambiguous and unsound.

Required package: core-js (via "core-js/modules/es.symbol")
Required by: /Users/jonjon/tmp/youp/storybook-yarn-berry-monorepo/packages/my-lib/stories/Button.tsx
```

It seems this can be resolved by installing `core-js` inside the workspace:

`yarn workspace my-lib add -D core-js`

## 2nd error: @storybook/core tried to access @storybook/addon-essentials

```
Failed to load preset: {"name":"@storybook/addon-essentials","type":"presets"} on level 1
ERR! Error: @storybook/core tried to access @storybook/addon-essentials, but it isn't declared in its dependencies; this makes the require call ambiguous and unsound.
ERR!
ERR! Required package: @storybook/addon-essentials (via "@storybook/addon-essentials")
ERR! Required by: @storybook/core@virtual:84a819bbc10d690fb0ef3885a34bf0a74bf459fcae5daaca157052b5be67adc87f7b6f165733c5fe1c828cc86d18ac7cada7aecd86e4b0df8123c04e6b95fc2c#npm:6.0.28 (via /Users/jonjon/tmp/youp/storybook-yarn-berry-monorepo/.yarn/$$virtual/@storybook-core-virtual-e39c3cd414/0/cache/@storybook-core-npm-6.0.28-47dac5a288-ee78dbe2de.zip/node_modules/@storybook/core/dist/server/)
```

It seems like installing `@storybook/addon-essentials` at the root of the
project works:

`yarn add -D @storybook/addon-essentials`

But now we got new errors, and we also have to install `webpack`,
`babel-loader`, `react-is`, and `@babel/core` at the root.

## At this point, there is no more errors but ...

But if you need to add loaders in a custom webpack config (e.g. `sass-loader`, `sass`),
Storybook will not work anymore if you install these dependencies in the
workspace.
Installing at the root of the project makes the errors disappear but it's not
ideal (from a monorepo perspective).
