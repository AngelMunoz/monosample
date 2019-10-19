# Monosample

[pnpm]: https://pnpm.js.org/
[pnpm docs]: https://pnpm.js.org/en/pnpm-workspace_yaml

This is a monorepo sample using [pnpm] while a bit overloaded with packages
it shows you how easy is to create a monorepo setup with [pnpm]


Each of the packages (`packages/**`) has it's own readme to explain
how to work inside that specific package

## Install
first install [pnpm]
- `npm i -g pnpm`

then run 
- `pnpm recursive install`
- `pnpm run build:lib`

after that you should be able to lift any of the applications you can check `package.json` on the root to see available commands

- `pnpm run dev:api2:website1`
- `pnpm run dev:api2:website2`
- `pnpm run dev:api2:website3`

these run the nestjs backend with Aurelia, Vue or Angular (respectively).

To run a command from a specific package.json, use [pnpm]'s filter commands

- `pnpm run dev --filter @monosample/api1`

that will run the `dev` command in the `@monosample/api1` package which resides in `packages/api1/package.json`, another example would be
- `pnpm run serve --filter @monosample/website2`

that will run the `serve` command in the `@monosample/website2` package which resides in `packages/website2/package.json`



## Root Setup
The following structure is used in this sample
```
|- package.json
|- .npmrc
|- pnpm-workspace.yaml
|- packages
    |- api1
    |- api2
    |- lib
    |- website1
    |- website2
    |- website3
```

>Important Notes:
> 1. inside `.npmrc` please use `shared-workspace-lockfile=false`, this allows us to take advantage of pnpm 4.0.0+ package hoisting
> 2. inside `pnpm-workspace.yaml` we setup the actual structure that will be followed by pnpm please refer to [pnpm docs]
> 3. If you are using webpack in one of your packages please set `{ resolve: { symlinks: false } }` this will prevent issues with webpack trying to locate the shared library packages (like `packages/lib`)
> 4. Not required, but recommended to use scoped package names in your projects to prevent name collisions example. `packages/lib/package.json`
>    ```javascript
>    {
>      "name": "@monosample/lib",
>      "version": "1.0.0",
>      "rivate": true,
>    }
>    // packages/api1/package.json
>    {
>     //...
>      "dependencies" : {
>         //...
>        "winston": "3.2.1",
>        "@monosample/lib": "1.0.0"
>         //...
>      },
>     //...
>    }
>    ```
> 5. try to use npm scripts on your packages to simplify development | deployment | builds
>    ```json
>    {
>      "scripts": {
>         "build:lib": "cd packages/lib && pnpm run build",
>         "build:lib:watch": "cd packages/lib && pnpm run build:watch"
>       }
>    }
>    ```
>    that way it's easier to keep on your root (`pnpm run build:lib`) terminal instead having a terminal open for each package. If it ever becomes too messy, remember you can always put javascript files on your root and use any utility library inside your scripts