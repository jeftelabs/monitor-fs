# Escopo e critérios de evolução

## Escopo observado

O projeto parseia configuração do TypeScript, mantém source files e usa Chokidar para emitir eventos separados para mudanças em scripts do projeto e em arquivos restantes. Os exports, testes e implementação atual confirmam esse escopo.

Este arquivo documenta o estado e os critérios de análise; não é uma lista de funcionalidades futuras comprometidas.

## Critérios para propostas

Ao propor mudanças, explicite:

1. Qual consumidor/caso precisa de comportamento novo.
2. Quais opções TypeScript e semântica de include/exclude serão respeitadas.
3. Extensões e regras de classificação, inclusive `allowJs` e exclusões.
4. Contratos e ordering de eventos, paths, opções Chokidar e ciclo de fechamento.
5. Compatibilidade com peer TypeScript, Node suportado e subpaths npm.
6. Testes de parser/source manager/watcher, docs, CI/build e impacto de release.

## Páginas para revisar por área

- Parser/config: `references/config-parser.md` e guia de setup.
- Globs/classificação: `references/source-files-manager.md` e guia de eventos.
- Chokidar/events: `references/watcher-and-events.md` e guia de eventos.
- Build/configuração: `references/package-and-build.md` e guia de desenvolvimento.
- Descoberta: `docs/README.md` e README raiz.

## Manutenção

Só registre roadmap como compromisso mediante decisão explícita do mantenedor. Ao alterar o escopo observado, valide as claims no código/testes e atualize esta página na mesma alteração.
