# Configurar e observar um projeto

## Parsear o `tsconfig`

TypeScript é peer dependency e o chamador passa seu módulo à classe:

```ts
import ts from 'typescript'
import { ConfigParser, Watcher } from '@jeftelabs/monitor-fs'

const root = process.cwd()
const { error, config } = new ConfigParser(root, 'tsconfig.json', ts).parse()
```

O primeiro argumento aceita string de caminho ou URL. O nome do config é relativo à raiz. `parse(optionsToExtend?)` entrega opções adicionais ao parser do TypeScript.

## Tratar diagnósticos

`error` contém diagnóstico fatal quando TypeScript aciona `onUnRecoverableConfigFileDiagnostic` (por exemplo, arquivo não encontrado); pode ser `null`. Erros de opções/sintaxe que o parser consegue reportar ficam em `config.errors`, mesmo quando `error` é null. `config` pode ser ausente se o arquivo não puder ser parseado.

```ts
if (error) {
  const host = ts.createCompilerHost({})
  console.error(ts.formatDiagnosticsWithColorAndContext([error], host))
} else if (!config) {
  throw new Error('Configuração indisponível')
} else if (config.errors.length > 0) {
  const host = ts.createCompilerHost({})
  console.error(ts.formatDiagnosticsWithColorAndContext(config.errors, host))
} else {
  const watcher = new Watcher(root, config)
  watcher.on('source:change', ({ relativePath }) => console.log('Recompilar:', relativePath))
  const fsWatcher = watcher.watch(['.'])
}
```

## Criar o watcher

`new Watcher(cwd: string, config: ts.ParsedCommandLine)` recebe raiz como string e resultado do parser. O método `watch(watchPattern = ['.'], watcherOptions?)` retorna `FSWatcher` do Chokidar. Opções padrão incluem `cwd: cwd`, `ignoreInitial: true` e `ignored` ligado aos padrões do SourceFilesManager. As opções fornecidas pelo usuário são combinadas sobre esses defaults, podendo sobrescrevê-los.

Ouça `watcher:ready` para saber quando Chokidar finalizou o scan inicial. Eventos de arquivo estão descritos no [guia de eventos](./events-and-classification.md).

## Encerrar observação

Como `watch()` retorna o watcher Chokidar, use o método `close()` documentado pelo Chokidar para liberar a observação e timers/handles relacionados. A aplicação cliente é responsável por encerrar o watcher em sua rotina de shutdown.

## Logs de diagnóstico

O módulo usa `node:util` `debuglog('chokidar:ts')`. Em ambientes Node que respeitam `NODE_DEBUG`, `NODE_DEBUG=chokidar:ts` habilita logs internos. O script `npm test` usa essa variável no comando atual.
