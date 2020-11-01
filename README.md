# storybook-yarn-berry-monorepo

Repro case for: Yarn v2 monorepo + Storybook with custom webpack config (for SASS support)

https://github.com/storybookjs/storybook/issues/9527#issuecomment-720045441

## Steps

- Clone repo

- Install dependencies

`yarn install`

- Start storybook

`yarn workspace my-lib storybook`

## Error: @storybook/core tried to access sass-loader

```
Module not found: Error: @storybook/core tried to access sass-loader, but it isn't declared in its dependencies; this makes the require call ambiguous and unsound.

Required package: sass-loader (via "sass-loader")
Required by: @storybook/core@npm:6.1.0-alpha.34 (via /Users/jonjon/tmp/storybook-yarn-berry-monorepo/.yarn/cache/@storybook-core-npm-6.1.0-alpha.34-a93cd16488-2d6e16aa2a.zip/node_modules/@storybook/core/)
```

Installing `sass-loader` and `sass` at the root of the project makes the error
go away:

`yarn add -D sass-loader sass`
