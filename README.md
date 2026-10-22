# `@jeftelabs/monitor-fs`

Uma camada fina sobre [Chokidar](https://github.com/paulmillr/chokidar) que usa a configuração do TypeScript para distinguir eventos de arquivos-fonte do projeto de eventos de arquivos comuns ou excluídos.

## Instalação

O pacote requer Node.js `>=18.16.0` e TypeScript `^5.0.0` como peer dependency:

```sh
npm install @jeftelabs/monitor-fs typescript
```

## Uso

```ts
import ts from 'typescript'
import { ConfigParser, Watcher } from '@jeftelabs/monitor-fs'

const projectRoot = process.cwd()
const parser = new ConfigParser(projectRoot, 'tsconfig.json', ts)
const { error, config } = parser.parse()

if (error) {
  console.error(ts.flattenDiagnosticMessageText(error.messageText, '\n'))
} else if (config?.errors.length) {
  const host = ts.createCompilerHost({})
  console.error(ts.formatDiagnosticsWithColorAndContext(config.errors, host))
} else if (config) {
  const watcher = new Watcher(projectRoot, config)

  watcher.on('source:add', (file) => console.log('TypeScript adicionado:', file.relativePath))
  watcher.on('source:change', (file) => console.log('TypeScript alterado:', file.relativePath))
  watcher.on('source:unlink', (file) => console.log('TypeScript removido:', file.relativePath))
  watcher.on('add', (file) => console.log('Outro arquivo adicionado:', file.relativePath))
  watcher.on('change', (file) => console.log('Outro arquivo alterado:', file.relativePath))
  watcher.on('unlink', (file) => console.log('Outro arquivo removido:', file.relativePath))
  watcher.on('watcher:ready', () => console.log('Watcher pronto'))

  const chokidarWatcher = watcher.watch(['.'])
}
```

`ConfigParser` recebe a raiz do projeto, o nome do arquivo de configuração e o módulo `typescript`. `Watcher` recebe a raiz como string e o `ParsedCommandLine` retornado pelo parser. O método `watch()` retorna o watcher do Chokidar e aceita padrões e opções Chokidar.

## Eventos

Eventos com prefixo `source:` indicam arquivos `.ts`/`.tsx` (e `.js` com `allowJs`) que pertencem ao projeto segundo `files`, `include` e `exclude`. Eventos sem prefixo são para outros arquivos ou caminhos excluídos. Todos os eventos de arquivo carregam `{ absPath, relativePath }`, com `relativePath` em formato Unix.

| Evento | Quando ocorre |
| --- | --- |
| `add` / `source:add` | Arquivo novo observado, comum ou fonte. |
| `change` / `source:change` | Arquivo observado foi alterado. |
| `unlink` / `source:unlink` | Arquivo observado foi removido. |
| `watcher:ready` | Chokidar terminou a varredura inicial. |

## API e documentação

O entry point exporta `ConfigParser`, `SourceFilesManager` e `Watcher`. Os tipos `SourceFilesManagerOptions` e `WatcherEvents` estão no subpath `@jeftelabs/monitor-fs/types`.

Consulte [`docs/README.md`](./docs/README.md) para o fluxo de configuração, regras de classificação, defaults, referências de API e desenvolvimento. As instruções de manutenção para agentes estão em [`AGENTS.md`](./AGENTS.md).

## Desenvolvimento

```sh
npm install
npm run quick:test
npm run lint
npm run typecheck
npm run build
```

`npm test` executa lint e testes com cobertura/debug; `npm run format` escreve formatação nos arquivos. Veja o [guia de desenvolvimento](./docs/guides/development.md).

## Licença

MIT. Consulte [`LICENSE`](./LICENSE).
