# Reproducer for Auto-Import bug in monorepos

This repository reproduces a TypeScript bug that occurs on monorepos.

In a scenario where you have one package that only contains types, and use this as a "devDependency"
in other packages in the monorepo, TypeScript is not able to suggest auto-imports for types from this
package. This works just fine if the dependency is added as a normal "dependency".

## Project structure

This is a pnpm monorepo with 3 packages:
- consumer-dependency
- consumer-dev-dependency
- lib

The consumer packages have a "workspace:*" dependency or devDependency respectively on the "lib" package.
The "lib" package exposes a single interface `MyInterface` from `src/the-lib.ts` using `package.json`
exports. This interface is used in a ts file within `consumer-dependency` and `consumer-dev-dependency`. 

## Reproduce

### Setup
* open the project in vscode
* run `pnpm install`

### Working scenario
* open `packages/consumer-dependency/src/dep-consumer.ts`
* move your caret to `MyInterface` and press `ctrl + .` to open suggestions
TSC suggests to add an import from "lib/the-lib" which is correct.

### Broken scenario
* open `packages/consumer-dev-dependency/src/dev-consumer.ts`
* move your caret to `MyInterface` and press `ctrl + .` to open suggestions
TSC doesn't suggest any imports.



