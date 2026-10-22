# Eventos e classificação de arquivos

## Tipos de evento

`Watcher` usa Emittery para expor eventos tipados:

| Evento | Significado |
| --- | --- |
| `add` | Arquivo novo observado que não foi classificado como fonte do projeto. |
| `source:add` | Arquivo novo reconhecido como fonte do projeto. |
| `change` | Arquivo existente não classificado como fonte foi alterado. |
| `source:change` | Arquivo-fonte existente foi alterado. |
| `unlink` | Arquivo não-fonte foi removido. |
| `source:unlink` | Arquivo-fonte foi removido. |
| `watcher:ready` | Chokidar concluiu a leitura inicial dos paths. Não tem payload. |

Eventos de arquivo recebem `{ absPath: string; relativePath: string }`. `relativePath` é normalizado para separadores Unix; `absPath` é criado com `join(cwd, path)`.

## Como um arquivo vira source

Watcher só considera tipos de script reconhecidos:

- `.ts` e `.tsx` sempre entram na classificação de extensões;
- `.js` entra se `config.options.allowJs` for truthy;
- outras extensões não são classificadas como fonte, mesmo se glob/`files` as incluir.

Para extensão reconhecida, `SourceFilesManager.isSourceFile(absPath)` retorna true quando:

1. O caminho está na lista `config.fileNames` carregada no começo; essa lista explícita é verificada primeiro, mesmo se o glob excluir o caminho.
2. Ou o caminho corresponde a pelo menos um `include` e não corresponde a nenhum `exclude`.

Padrões são unidos à raiz e convertidos para slash antes de criar matchers `picomatch`. Os caminhos recebidos nas consultas também são convertidos para slash.

## Eventos de criação e remoção atualizam o conjunto

Quando um arquivo novo é `.ts`/`.tsx` ou JS permitido e passa pelos padrões, o watcher chama `SourceFilesManager.add()` antes de emitir `source:add`. Ao remover uma source, chama `remove()` antes de `source:unlink`. Alterações não alteram a lista.

Isso mantém em memória o conjunto inicial de `fileNames` e mudanças source adicionadas/removidas enquanto o watcher está ativo. Não há reparse automático do `tsconfig`.

## Filtros de paths observados

O predicado `ignored` padrão do Chokidar ignora paths que `shouldWatch()` classifica como fora do projeto. A raiz sempre passa. O matcher considera globs include/exclude; arquivos explícitos são registrados como fonte. Padrões padrão e substituição de excludes estão detalhados na [visão geral](./overview.md) e na referência do watcher.

## Consumidores

Uma ferramenta pode recompilar/reiniciar em `source:add`, `source:change` ou `source:unlink`, e tratar em separado assets, config de runtime ou arquivos ignorados via `add/change/unlink`. O pacote não decide essas ações.
