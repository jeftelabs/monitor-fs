# Documentação de `@jeftelabs/monitor-fs`

Esta documentação descreve os contratos da implementação local e dos testes. O parser TypeScript, Chokidar, código e testes do repositório são as fontes definitivas em caso de divergência.

## Comece aqui

- [Visão geral e arquitetura](./guides/overview.md): responsabilidades e fluxo dos componentes.
- [Configurar e observar um projeto](./guides/watching-a-project.md): integração do parser com o watcher.
- [Interpretar eventos](./guides/events-and-classification.md): eventos, payloads e classificação source/non-source.
- [Desenvolvimento](./guides/development.md): testes, configuração, build e publicação.

## Guias

| Documento | Conteúdo |
| --- | --- |
| [Visão geral](./guides/overview.md) | Objetivo, componentes e ciclo de vida. |
| [Observar um projeto](./guides/watching-a-project.md) | ConfigParser, diagnósticos, Watcher e opções Chokidar. |
| [Eventos e classificação](./guides/events-and-classification.md) | Como `tsconfig` e extensões determinam os eventos. |
| [Desenvolvimento](./guides/development.md) | Estrutura, scripts, testes e publicação. |

## Referências

| Documento | Conteúdo |
| --- | --- |
| [ConfigParser](./references/config-parser.md) | Construtor, `parse`, diagnósticos e configuração. |
| [SourceFilesManager](./references/source-files-manager.md) | Padrões, estado e métodos de classificação. |
| [Watcher e eventos](./references/watcher-and-events.md) | Opções, defaults, eventos e payloads. |
| [Pacote e build](./references/package-and-build.md) | Exports, peer dependency, `tsconfig` e workflows. |
| [Testes](./references/tests.md) | Cobertura existente e forma de verificar contratos. |

## Planning

| Documento | Conteúdo |
| --- | --- |
| [Escopo e critérios de evolução](./planning/roadmap.md) | Escopo observado e critérios para propostas sem compromissos inventados. |

## Para agentes de IA

Leia [`../AGENTS.md`](../AGENTS.md) e [`../CLAUDE.md`](../CLAUDE.md). Toda criação ou modificação do projeto requer documentação detalhada na mesma tarefa; inclua novas páginas neste índice.
