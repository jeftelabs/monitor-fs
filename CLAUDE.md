# Orientações para Claude e agentes de IA

As instruções canônicas estão em [`AGENTS.md`](./AGENTS.md). Leia também as páginas relevantes de [`docs/README.md`](./docs/README.md). Este repositório contém `@jeftelabs/monitor-fs`, uma integração de Chokidar com o parser oficial de `tsconfig` do TypeScript.

## Fluxo de investigação

1. Leia `AGENTS.md` e a documentação da área.
2. Confira o parser, o manager e o watcher em `src/`, junto aos testes relacionados.
3. Verifique exports em `index.ts` e `package.json`, configuração TypeScript, dependências/peer dependencies e workflows.
4. Use README.md como introdução pública e valide exemplos/comportamento no código e nos testes locais.

## Documentação obrigatória para toda alteração

É **extremamente obrigatório criar ou atualizar documentação detalhada sempre que algo novo for criado ou qualquer parte do projeto for modificada**. Isso inclui código, API, tipos, testes, configuração, dependências, scripts, automações, exemplos e os documentos em si. Faça a atualização na mesma tarefa.

- Explique para humanos e IAs por que existe, como funciona, quais opções/efeitos/eventos produz, limites e como manter/verificar.
- Mantenha páginas em `docs/` com `guides`, `references`, `planning` e seções pertinentes.
- Inclua todas as páginas novas no índice `docs/README.md` e mantenha descoberta a partir do README raiz.
- Atualize o README quando instalação, API, compatibilidade ou exemplos mudarem.
- Confira claims sobre TypeScript parser, globs, extensões e eventos na implementação. Diferencie erro fatal de `ParsedCommandLine.errors`.
- Documente qualquer alteração em configuração necessária ao runner ou build.
- Não finalize sem documentação correspondente.

## Mapa do pacote

- [`index.ts`](./index.ts): exports `ConfigParser`, `SourceFilesManager`, `Watcher`.
- `src/config_parser.ts`: invoca `typescript.getParsedCommandLineOfConfigFile`.
- `src/source_files_manager.ts`: normaliza paths e aplica glob patterns com picomatch.
- `src/watcher.ts`: filtra caminhos no Chokidar, classifica eventos e atualiza source manager.
- `src/types.ts`: opções e mapa de eventos tipados.
- `tests/`: testes de config parser e source manager; exemplos de uso ficam no README.

## Comandos e práticas

Confirme scripts em `package.json`: `npm run quick:test`, `npm test`, `npm run lint`, `npm run typecheck`, `npm run build` e `npm run format` (escreve). Preserve ESM, subpath `/types`, peer dependency TypeScript, normalização de paths e payloads de eventos.

Use Conventional Commits. Faça commit/push/PR apenas quando solicitado. Não descarte alterações existentes ou exponha segredos.
