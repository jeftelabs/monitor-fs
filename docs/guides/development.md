# Desenvolvimento e manutenção

## Pré-requisitos

O pacote usa Node.js `>=18.16.0`, npm e TypeScript `^5.0.0` como peer dependency. O lockfile registra ferramentas de desenvolvimento; instale com `npm ci` para reprodução ou `npm install` no desenvolvimento.

## Estrutura

- `index.ts`: exports públicos.
- `src/config_parser.ts`: leitura de config pelo compilador TypeScript.
- `src/source_files_manager.ts`: conjunto de arquivos e matching include/exclude.
- `src/watcher.ts`: integração com Chokidar e eventos.
- `src/types.ts`: opções e payloads.
- `tests/`: testes de parser e SourceFilesManager.
- `.github/workflows/`: commitlint, release, deploy e manutenção de issues/PRs.

## Comandos

| Comando | Efeito |
| --- | --- |
| `npm run quick:test` | Executa `bin/test.ts` pelo `@jeftelabs/exec`. |
| `npm test` | Roda lint (`pretest`) e quick:test com c8 e debug. |
| `npm run lint` | ESLint. |
| `npm run typecheck` | `tsc --noEmit`. |
| `npm run compile` | `tsup-node` mais declaração TypeScript. |
| `npm run build` | Executa compile; `precompile` roda lint e limpa `build`. |
| `npm run format` | Prettier escreve os arquivos. |

Verifique os scripts atuais no package.json antes de usá-los.

## Configuração TypeScript do repositório

`tsconfig.json` estende `@arapucajs/tsconfig/tsconfig.package.json`, configura `rootDir` e `outDir` como `build/` e ativa `rewriteRelativeImportExtensions`. Esta opção permite imports relativos `.ts` usados pelo runner serem reescritos na saída. Preserve-a enquanto esse padrão de import/runtime estiver vigente; alterações nesta configuração precisam ser documentadas.

## Verificações ao alterar comportamento

1. Atualize testes do `ConfigParser` para cenários de erro fatal versus `config.errors` e opções de TS.
2. Atualize testes do source manager para files/include/exclude e normalização de caminhos.
3. Para mudanças de watcher, verifique extensões, eventos, paths relativos/absolutos, defaults Chokidar e shutdown.
4. Atualize guias, referências e README na mesma tarefa.

No checkout atual não há testes dedicados ao `Watcher`; considere isso ao interpretar a cobertura do pacote.

## Build, CI e publicação

`tsup-node` empacota `index.ts` e `src/types.ts` para ESM; `tsc --emitDeclarationOnly --declaration` emite tipos. O pacote publica `build` e exporta a raiz e `./types`. `.husky/pre-commit` chama `npm test`.

`checks.yml` é acionado por push em `main` e valida Conventional Commits; há uma etapa condicional para PR, mas o workflow não declara evento `pull_request` atualmente. `release.yml` usa release-please em `main`, `deploy.yml` publica após release, e `labels.yml`/`stale.yml` tratam manutenção do GitHub.
