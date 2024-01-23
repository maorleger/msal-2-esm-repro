# Repro of incompatibility between MSAL 2.x and the ESM NPM package

This is a minimal repro showing different behaviors when loading MSAL 2.x:

Context: MSAL 2.x moved to ESM with [conditional exports](https://nodejs.org/api/packages.html#conditional-exports) controlling whether to load ESM or CJS

The problem is that a very popular package [esm](https://www.npmjs.com/package/esm) (2.6 million downloads per week) does not support conditional exports. As a result, using `esm` prevents the ability to load the correct type of MSAL module.

To run this:

```bash
npm install
npm run start:module # runs main.mjs using native esm, prints "ok" and MSAL's version
npm run start:commonjs # runs main.cjs using common js, prints "ok" and MSAL's version
npm run start:esm # runs main.cjs using the esm package, throws Error: Cannot find module '@azure/msal-node'
```
