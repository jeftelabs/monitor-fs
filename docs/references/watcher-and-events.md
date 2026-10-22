# Referência: `Watcher` e eventos

Implementação em [`src/watcher.ts`](../../src/watcher.ts). `Watcher` estende `Emittery<WatcherEvents & { 'watcher:ready': undefined }>`.

## Construtor

```ts
new Watcher(cwd: string, config: ts.ParsedCommandLine)
```

Constrói o `SourceFilesManager` com `config.fileNames`, `config.raw.include` e excludes derivados do config/destino de saída.

## `watch`

```ts
watch(watchPattern: string | string[] = ['.'], watcherOptions?: ChokidarOptions): FSWatcher
```

Defaults de Chokidar: `ignored` usa `!sourceFilesManager.shouldWatch(path)`, `cwd` é a raiz e `ignoreInitial: true`. `Object.assign(defaults, watcherOptions)` permite que options do usuário substituam qualquer um desses defaults. Eventos Chokidar `ready`, `add`, `change`, `unlink` são transformados/em itidos pelo `Watcher` e o `FSWatcher` é retornado para controle/fechamento.

## Classificação de extensão

- `.ts` e `.tsx`: candidatos a source.
- `.js`: candidato somente quando `config.options.allowJs` é truthy.
- demais extensões: sempre evento comum.

Para candidatos, source manager decide se o arquivo participa do projeto. `add` de source atualiza o manager antes do evento; `unlink` de source remove antes do evento. `change` não muda registro.

## Eventos e payloads

Tipos estão em [`src/types.ts`](../../src/types.ts) e no subpath `@jeftelabs/monitor-fs/types`.

- `add`, `source:add`, `change`, `source:change`, `unlink`, `source:unlink`: `{ absPath: string; relativePath: string }`.
- `watcher:ready`: `undefined`.

`relativePath` usa `slash()`. O evento `watcher:ready` ocorre após o evento `ready` de Chokidar.

## Padrões padrão

- include de fallback: `**/*`.
- excludes sempre: `.git/**`, `coverage/**`, `.github/**`.
- fallback de exclude: `node_modules/**`, `bower_components/**`, `jspm_packages/**`; se houver `outDir`, ele é anexado ao fallback.
- quando `config.raw.exclude` existe, ele é combinado aos excludes sempre, e substitui os defaults de dependências/outDir no ramo atual.

Os patterns vêm de `config.raw`, não de uma recomputação manual da semântica inteira do TypeScript.
