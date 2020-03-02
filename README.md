# async-fp

[![NPM version][npm-image]][npm-url]
[![NPM downloads][downloads-image]][downloads-url]

[![Github NodeJS][github-nodejs]][github-action-url]
[![Codecov][codecov-image]][codecov-url]
[![Coveralls Status][coveralls-image]][coveralls-url]
[![Codacy Badge][codacy-image]][codacy-url]

[![Greenkeeper][greenkeeper-image]][greenkeeper-url]
[![Semantic Release][semantic-release-image]][semantic-release-url]

[![Visual Studio Code][vscode-image]][vscode-url]
[![Wallaby.js][wallaby-image]][wallaby-url]

async functional programming library.

## Installation

```sh
npm install async-fp
// or
yarn add async-fp
```

## AsyncContext

It is common to pass in a context object containing dependencies used by the function.
In some cases, the dependencies needs to be loaded asynchronously. For example,

- code is imported and loaded dynamically
- some async work needs to be done before the dependency is available

When your code is invoked by other code where you cannot control its timing,
you need a mechanism to wait for the dependencies.

```ts
import { AsyncContext } from 'async-fp'

const ctx = new AsyncContext()

someFunc(ctx, 'hello world')
ctx.set(async () => ({ io: await createIO() }))

async function someFunc(context: AsyncContext<{ io: PartOfIO }>, msg: string) {
  const { io } = await context.get()
  io.write(msg)

  someOtherFunc(context.merge(async () => ({ ui: await createUI() }), 'bye world')
}

async function someOtherFunc(
  context: Context<{ io: AnotherPartOfIO, ui: SomeUI }>,
  msg: string
) {
  const { io, ui } = await context.get()
  const data = io.read()
  ui.info(data, msg)
}
```

- `new AsyncContext<T>(context?: T | (() => T | Promise<T>))`: Create a new async context object.
- `AsyncContext#set(context: T | (() => T | Promise<T>)`: Set context value. This is used when the context is created the producer while `set()` is called by consumer.
- `AsyncContext#merge(context: T | ((currentContext) => T | Promise<T>)`: Merge new context input to create a new async context object.
- `AsyncContext#clear()`: Clear the context as if the context is created with no context argument during creation or with `set()` method calls. Used mostly for testing.

[codacy-image]: https://api.codacy.com/project/badge/Grade/569e678c65cf4481a172aaeb83b41aef
[codacy-url]: https://www.codacy.com/app/homawong/async-fp?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=unional/async-fp&amp;utm_campaign=Badge_Grade
[codecov-image]: https://codecov.io/gh/unional/async-fp/branch/master/graph/badge.svg
[codecov-url]: https://codecov.io/gh/unional/async-fp
[coveralls-image]: https://coveralls.io/repos/github/unional/async-fp/badge.svg
[coveralls-url]: https://coveralls.io/github/unional/async-fp
[downloads-image]: https://img.shields.io/npm/dm/async-fp.svg?style=flat
[downloads-url]: https://npmjs.org/package/async-fp
[github-nodejs]: https://github.com/unional/async-fp/workflows/Node%20CI/badge.svg
[github-action-url]: https://github.com/unional/async-fp/actions
[greenkeeper-image]: https://badges.greenkeeper.io/unional/async-fp.svg
[greenkeeper-url]: https://greenkeeper.io/
[npm-image]: https://img.shields.io/npm/v/async-fp.svg?style=flat
[npm-url]: https://npmjs.org/package/async-fp
[semantic-release-image]: https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg
[semantic-release-url]: https://github.com/semantic-release/semantic-release
[vscode-image]: https://img.shields.io/badge/vscode-ready-green.svg
[vscode-url]: https://code.visualstudio.com/
[wallaby-image]: https://img.shields.io/badge/wallaby.js-configured-green.svg
[wallaby-url]: https://wallabyjs.com
