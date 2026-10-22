# Referência: `ConfigParser`

Implementação em [`src/config_parser.ts`](../../src/config_parser.ts), exportada na raiz.

## Construtor

```ts
new ConfigParser(cwd: string | URL, configFileName: string, ts: typeof import('typescript'))
```

- `cwd`: raiz do projeto; URL é convertida por `fileURLToPath`.
- `configFileName`: nome/caminho relativo unido à raiz.
- `ts`: instância/módulo TypeScript fornecida pelo chamador.

## `parse(optionsToExtend?)`

```ts
parse(optionsToExtend?: ts.CompilerOptions): {
  error: ts.Diagnostic | null
  config?: ts.ParsedCommandLine
}
```

Invoca `ts.getParsedCommandLineOfConfigFile` com opções extras ou `{}` e host baseado em `ts.sys`; substitui `getCurrentDirectory` pela raiz do projeto e define comportamento de case-sensitive.

`error` captura o diagnóstico enviado ao callback `onUnRecoverableConfigFileDiagnostic`. `config.errors` carrega diagnósticos recuperáveis retornados no `ParsedCommandLine`, por exemplo opção desconhecida. Verifique ambos os resultados; `error === null` não garante `config.errors` vazio.

O parser não imprime diagnósticos nem lança erro de config por conta própria. O consumidor escolhe como formatar/mostrar erros, usando APIs TypeScript como `formatDiagnosticsWithColorAndContext`.
