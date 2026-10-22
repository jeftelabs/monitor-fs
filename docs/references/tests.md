# Referência dos testes

## Runner

[`bin/test.ts`](../../bin/test.ts) configura `@t8ngs/runner`, os plugins de assertions e filesystem e procura `tests/**/*.spec.ts`. `npm run quick:test` executa com `@jeftelabs/exec`. `npm test` roda lint por `pretest` e então `cross-env NODE_DEBUG=chokidar:ts c8 npm run quick:test`, produzindo relatórios text/html configurados em `c8`.

## Casos existentes

- [`tests/config_parser.spec.ts`](../../tests/config_parser.spec.ts): config ausente, opção inválida, coleta de files por include, exclude e precedence de `files` explícito.
- [`tests/source_files_manager.spec.ts`](../../tests/source_files_manager.spec.ts): include/exclude, paths, adição/remoção, pertença ao conjunto explícito e `shouldWatch` para raiz/arquivos/diretórios.

Não existe teste `Watcher` no estado atual. A integração real com Chokidar e o mapeamento de eventos não têm cobertura direta nesses arquivos.

## Manutenção

Ao mudar parser, teste tanto `error` fatal quanto `config.errors`. Ao mudar matcher, cubra padrões e normalização multiplataforma. Ao alterar Watcher, inclua casos de eventos, extensões (`allowJs`), defaults/overrides de Chokidar e encerramento do watcher; atualize esta página e as referências.
