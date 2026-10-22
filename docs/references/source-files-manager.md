# Referência: `SourceFilesManager`

Implementação em [`src/source_files_manager.ts`](../../src/source_files_manager.ts). Recebe raiz e `SourceFilesManagerOptions` (`files: string[]`, `includes?: string[]`, `excludes?: string[]`).

## Inicialização

Converte raiz para slash e remove barra final. Os caminhos de `files` são adicionados ao mapa `#projectFiles` antes da criação dos matchers; esses são os arquivos de projeto explícitos retornados por TypeScript.

Padrões de include/exclude são resolvidos com `join(appRoot, pattern)`, normalizados em slash e compilados por `picomatch`. Include/exclude ausentes viram arrays vazios para os matchers desta classe.

## Métodos

- `add(filePath)`: normaliza para slash e registra o caminho como arquivo de projeto.
- `remove(filePath)`: remove o caminho do registro explícito.
- `isSourceFile(filePath)`: verdadeiro se estiver no registro explícito ou se passar o matcher de include e não passar o de exclude.
- `shouldWatch(filePath)`: sempre aceita a raiz exata; para outros caminhos delega a `isSourceFile`.
- `toJSON()`: retorna o registro interno de arquivos explícitos; no código atual não cria cópia apesar do comentário legado.

Resultados por padrão são memoizados. `add`/`remove` alteram o registro explícito, que é consultado antes do cache de padrões.

## Observações

Os métodos recebem caminhos absolutos no uso pelo `Watcher`, pois o watcher combina `cwd` com caminhos relativos do Chokidar. A classe não verifica extensão: a regra de extensão fica em `Watcher`. As regras de exclusão para o watcher também incluem defaults calculados em `Watcher`, não nesta classe.
