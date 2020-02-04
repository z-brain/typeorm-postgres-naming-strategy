# Z-Brain TypeORM PostgreSQL Naming Strategy

Pascal/Camel Case naming for everything in the PostgreSQL

*Notice: If you have any propositions feel free to make an issue or create a pull request.*

## Features

* All constraint names appended with 8-char hash for uniqueness. Example: `eed18e0e`
* All names trimmed to 63 bytes including 8-char hash if the name is too long (in most cases length === bytes number, however not always).

Naming entity       | Case                                                  | Examples
--------------------|-------------------------------------------------------|--------
Table               | `PascalCase`                                          | `Users`, `MySuperTable`
Column              | `CamelCase`                                           | `id`, `mySuperColumn`
Enum                | (not supported by TypeORM yet)                        |
Primary Key         | `PK_{table}_{cols}_{hash}`                            | `PK_Instruments_id_bd441074`, `PK_MySuperTable_email,pwdHash_d1d1d1d1`
Unique Constrain    | `UQ_{table}_{cols}_{hash}`                            | `UQ_Instruments_code_051d8d38`, `UQ_MySuperTable_firstName,lastName_d1d1d1d1`
Default Constrain   | `DF_{table}_{col}_{hash}`                             | `UQ_Users_email`, `DF_MySuperTable_firstName`
Relation Constrain  | `REL_{table}_{cols}_{where}_{hash}`                   | TODO
Check Constrain     | `CHK_{table}_{expression}_{hash}`                     | TODO
Exclusion Constrain | `XCL_{table}_{expression}_{hash}`                     | TODO
Foreign Key         | `FK_{table}_{targetTable}_{cols}_{targetCols}_{hash}` | `FK_TradingPairs_Instruments_quotedInstrumentId_id_fc68de3f`
Index               | `IDX_{table}_{cols}_{hash}`                           | `IDX_Tickers_exchangeId,symbol_c8090854`
Unique Index        | `UQIDX_{table}_{cols}_{hash}`                         | `UQIDX_Tickers_exchangeId,symbol_c8090854`

## How to use

### Installing

`yarn add @z-brain/typeorm-postgres-naming-strategy`  
or  
`yarn add @z-brain/typeorm-postgres-naming-strategy`

### Configuring

`/ormconfig.ts`
```typescript
import { TypeORMPostgresNamingStrategy } from '@z-brain/typeorm-postgres-naming-strategy';

// Store an instance separately for reuse methods in you app
export const typeORMNamingStrategy = new TypeORMPostgresNamingStrategy();

// TypeORM connection config for PostgreSQL
export const defaultConnection: ConnectionOptions = {
  // ...
  namingStrategy: typeORMNamingStrategy,
};
```

## Development notes

### Quick Start

```bash
cd /code/z-brain
git clone git@github.com:z-brain/typeorm-postgres-naming-strategy.git
cd typeorm-postgres-naming-strategy
yarn install
```

### How to use NodeJS version from the `.nvmrc`

1. Install NVM
2. Use `.nvmrc` file one of the next ways:

    * Execute `nvm use` in the project root directory
    * Install [NVM Loader](https://github.com/korniychuk/ankor-shell) and your .nvmrc will be loaded automatically when you open the terminal.
      ![NVM Loader demo](./resources/readme.nvm-loader.png)

### How to make a build

`npm run build`

### How to run lint

* Just show problems `npm run lint`
* Fix problems if it is possible `npm run lint:fix`

### How to run tests

* All tests

  `npm run test`  
  `npm run test:watch`
* Specific tests

  `npm run test -- src/my.spec.ts`  
  `npm run test:watch -- src/my.spec.ts`

### How to build and publish NPM package

*NPM Token:* `367a...ce73`

CI configuration details here: [.github/workflows/npmpublish.yml](.github/workflows/npmpublish.yml)

```bash
npm run pre-push
&& npm version patch -m 'Update package version version to %s'
&& npm run gen-public-package.json
&& cp README.md dist/
&& npm publish dist --access public
&& git push --no-verify && git push --tags --no-verify
```

## Author

| [<img src="https://www.korniychuk.pro/avatar.jpg" width="100px;"/><br /><sub>Anton Korniychuk</sub>](https://korniychuk.pro) |
| :---: |
