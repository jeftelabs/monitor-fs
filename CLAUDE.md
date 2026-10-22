# Instruções para Claude e agentes de IA

As regras completas e canônicas do repositório estão em [`AGENTS.md`](./AGENTS.md). Leia esse arquivo e a documentação pertinente em [`docs/README.md`](./docs/README.md) antes de trabalhar. Este repositório contém `@jeftelabs/exception`, uma biblioteca ESM em TypeScript para erros personalizados.

## Fluxo de entendimento

1. Confira `AGENTS.md` e os guias/referências em `docs/`.
2. Confirme o comportamento em [`src/exception.ts`](./src/exception.ts), a API pública em [`index.ts`](./index.ts) e os casos existentes em `tests/`.
3. Consulte `package.json`, `tsconfig.json` e `.github/workflows/` para validar os entry points, os scripts, o build e as rotinas de publicação.
4. Use [`README.md`](./README.md) para orientação de usuários. `README copy.md` é apenas uma referência editorial: não copie descrições ou exemplos sem conferi-los contra este pacote.

## Documentação obrigatória para toda alteração

É **extremamente obrigatório criar ou atualizar documentação detalhada toda vez que algo novo for criado ou qualquer parte do projeto for modificada**. Isso inclui código, APIs, testes, exemplos, configuração, dependências, scripts, CI/CD e os próprios documentos. Complete a documentação na mesma tarefa da alteração.

- Registre em `docs/` o propósito, o funcionamento, o uso, os efeitos, as limitações e a manutenção da área afetada.
- Atualize `docs/README.md` para que toda página nova seja fácil de encontrar, usando a organização em guias, referências, planning e outras seções relevantes.
- Atualize o README raiz quando houver impacto em instalação, API pública, uso, exemplos, requisitos ou contribuição.
- Use assinaturas e exemplos confirmados na implementação e nos testes; não atribua ao pacote comportamentos de outro projeto ou do arquivo de referência.
- Não conclua a tarefa deixando documentação necessária para depois.

## Resumo da API e do projeto

- `Exception` é a classe base e aceita mensagem e `ErrorOptions` estendidas com `code` e `status`.
- `InvalidArgumentsException` e `RuntimeException` são subclasses pré-configuradas.
- `createError` cria uma classe de exceção com mensagem e código estáticos; os argumentos para interpolação são passados como array.
- O entry point único é `index.ts`; a distribuição ESM e as declarações são geradas em `build/`.
- O runner de teste começa em `bin/test.ts`; os casos estão em `tests/exception.spec.ts`.

## Comandos e práticas

Verifique `package.json` para os scripts atuais. `npm test` inclui lint e cobertura; `npm run quick:test` executa testes sem cobertura; `npm run lint`, `npm run typecheck` e `npm run build` fazem as verificações descritas nos scripts. `npm run format` altera os arquivos.

Preserve as exportações, a compatibilidade TypeScript/ESM e a semântica de erro coberta pelos testes. Mantenha imports relativos TypeScript compatíveis com `@jeftelabs/exec` via `rewriteRelativeImportExtensions`. Use Conventional Commits. Faça commit, push e PR apenas quando houver solicitação explícita. Nunca descarte alterações existentes ou exponha segredos.
