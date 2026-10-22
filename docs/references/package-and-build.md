# Referência de pacote e build

## Exports públicos

[`package.json`](../../package.json) declara `@jeftelabs/monitor-fs`, ESM, `main: build/index.js`, Node `>=18.16.0`, TypeScript `^5.0.0` como peer dependency. Exports: `.` para `build/index.js` e `./types` para `build/src/types.js`. O entry fonte `index.ts` exporta `Watcher`, `ConfigParser` e `SourceFilesManager`.

`files` inclui `build`, excluindo `build/bin` e `build/tests`.

## TypeScript e imports

[`tsconfig.json`](../../tsconfig.json) estende `@arapucajs/tsconfig/tsconfig.package.json`, define `rootDir: ./`, `outDir: ./build` e `rewriteRelativeImportExtensions: true`. Essa opção acompanha imports relativos `.ts` executados pelo loader de desenvolvimento e substituídos pelo TypeScript no output.

## Build

`tsup`/`tsup-node` usa `index.ts` e `src/types.ts`, destino `build`, ESM, limpa output e target `esnext`. O script `compile` roda bundling mais `tsc --emitDeclarationOnly --declaration`; `precompile` roda lint e clean. `prepublishOnly` roda build.

## Dependências relevantes

- Peer: `typescript`, fornecido pelo consumidor e usado pelo `ConfigParser`/Watcher.
- Runtime: `chokidar` para filesystem events, `emittery` para API de eventos, `picomatch` para glob matching, `slash` para paths multiplataforma, `memoize` para decisões de matching.
- Dev: `@jeftelabs/exec`, `@t8ngs/*`, `c8`, ESLint/Prettier, TypeScript e ferramentas de build/release.

## Workflows

- `.husky/pre-commit`: `npm test`.
- `checks.yml`: push em `main` com validação de Conventional Commits; contém step condicional para PR, mas não declara evento `pull_request` atualmente.
- `release.yml`: release-please após push para `main`, pode gerar/anexar tarball.
- `deploy.yml`: publica em GitHub Packages e npm após release publicada.
- `labels.yml` e `stale.yml`: manutenção de labels/issues/PRs.

Confira os arquivos de configuração ao atualizá-los; não suponha etapas de CI que não estão declaradas.
