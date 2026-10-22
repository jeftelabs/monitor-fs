# Visão geral e arquitetura

## Propósito

`@jeftelabs/monitor-fs` observa um diretório usando Chokidar e classifica mudanças pelo contexto de compilação do TypeScript. Isso permite que uma ferramenta que compila/reinicia a aplicação reaja de maneira diferente a mudanças em arquivos que pertencem ao projeto TypeScript e a mudanças em arquivos restantes.

O pacote não executa compilação nem reinicia processos. Ele lê a configuração, mantém estado de caminhos-fonte e emite eventos; a aplicação cliente decide o que fazer com eles.

## Componentes

- [`index.ts`](../../index.ts) exporta `ConfigParser`, `SourceFilesManager`, `Watcher`.
- [`src/config_parser.ts`](../../src/config_parser.ts) chama `typescript.getParsedCommandLineOfConfigFile` com raiz e nome de arquivo fornecidos.
- [`src/source_files_manager.ts`](../../src/source_files_manager.ts) normaliza caminhos com `slash`, combina arquivos explícitos com glob `include`/`exclude` via `picomatch` e memoiza decisões por padrão.
- [`src/watcher.ts`](../../src/watcher.ts) configura Chokidar, filtra caminhos, classifica arquivos por extensão e participação no projeto, atualiza o manager e emite eventos tipados via Emittery.
- [`src/types.ts`](../../src/types.ts) declara opções do source manager e payloads de eventos.

## Fluxo geral

1. A aplicação passa o módulo TypeScript ao `ConfigParser`.
2. `parse()` retorna um diagnóstico fatal opcional e, se possível, um `ParsedCommandLine`; diagnósticos recuperáveis ficam em `config.errors`.
3. A aplicação passa a raiz e o config parseado a `Watcher`.
4. O watcher cria um `SourceFilesManager` com `config.fileNames`, `include` e `exclude`.
5. `watch()` cria um watcher Chokidar com raiz (`cwd`), ignora eventos iniciais e um predicado `ignored` que filtra caminhos fora dos padrões, salvo opções fornecidas pelo chamador.
6. Arquivos `.ts`/`.tsx` e `.js` quando `allowJs` está ativo podem ser emitidos em eventos `source:*` se reconhecidos pelo manager; os demais usam eventos simples.

## Defaults principais

- Include: `**/*` se `config.raw.include` não estiver definido.
- Sempre excluídos: `.git/**`, `coverage/**`, `.github/**`.
- Se o config não define excludes, são excluídos `node_modules/**`, `bower_components/**`, `jspm_packages/**` e, quando há `outDir`, esse diretório.
- Se o config define `exclude`, ele é combinado com os excludes sempre ativos; os excludes padrão de dependências/outDir não são acrescentados pelo ramo atual.
- Raiz do projeto sempre é observada pelo manager.

Confira [eventos e classificação](./events-and-classification.md) para precedência entre `files`, includes/excludes e extensões.

## Limites

O watcher não classifica `.mts`, `.cts`, `.jsx` nem outros formatos; `.js` depende de `allowJs`. Ele usa os valores `raw.include`, `raw.exclude`, `raw.compilerOptions.outDir`, `options.allowJs` e `fileNames` produzidos por TypeScript. Se o arquivo de config mudar enquanto está rodando, esta implementação não o relê automaticamente.
