# Laboratório — GitHub Actions — publicação de artefatos

- Registrado e pesquisado em: 2026-10-07
- Alteração inicial identificada: 2026-10-04
- Situação: geração e upload configurados; artefato publicado não confirmado.

## Pergunta do experimento

Como preservar um arquivo produzido por um job para consulta após a execução?

## Ambiente e evidências

Projeto pessoal de Ewerton: [devsecops-with-github-actions](https://github.com/carreiras/devsecops-with-github-actions). Checkout local: `C:\projetos\estudos\devsecops-with-github-actions`; snapshot `55fe6ed4fb8095ba4d82e0caacdb5dce7583a2e6`.

Configuração no job `test` de `.github/workflows/main.yaml`, em `ubuntu-latest`. `actions/upload-artifact@v4` é a referência existente; não foi verificada a versão resolvida numa execução.

## Passos executados

O commit [c29a42d](https://github.com/carreiras/devsecops-with-github-actions/commit/c29a42d43977ebffec44c9f126d754055be56ae9), de 2026-10-04, comprova a inclusão destas etapas de configuração, na ordem abaixo:

1. Acrescentado, após o step que chama a action, o step `File` com `echo "Test" > test.txt`.
2. Acrescentado o step chamado `Download`, que utiliza `actions/upload-artifact@v4` para fazer upload.
3. Definido `with.path: test.txt`, ligando o arquivo produzido pelo step anterior à action de upload.

No snapshot analisado, esses steps permanecem nessa ordem. Como foram acrescentados no mesmo commit, essa sequência descreve a ordem configurada, não a ordem exata das edições no computador do usuário.

**Não reconstruído:** geração efetiva de `test.txt` num runner, publicação, download e inspeção de um artefato remoto. Não há logs ou arquivo baixado disponíveis para confirmar esses passos de execução.

## Configuração identificada

Após chamar a action Soma, o job contém:

```yaml
- name: File
  run: echo "Test" > test.txt
- name: Download
  uses: actions/upload-artifact@v4
  with:
    path: test.txt
```

O comando grava `Test` e uma quebra de linha em `test.txt`. A action seguinte envia o arquivo como artefato. Apesar do nome `Download`, o step configura um **upload**; o rótulo não altera o comportamento. O trecho pertence aos steps do job e não é um workflow completo. [GitHub — upload-artifact](https://github.com/actions/upload-artifact).

## Evolução comprovada

O commit [c29a42d](https://github.com/carreiras/devsecops-with-github-actions/commit/c29a42d43977ebffec44c9f126d754055be56ae9), de 2026-10-04, acrescenta criação do arquivo e upload. O snapshot mantém os steps. Isso comprova configuração, não um artefato disponível para download no GitHub.

## Resultado esperado e observado

| Etapa | Esperado | Evidência disponível |
|---|---|---|
| Geração | `test.txt` contendo `Test` | Comando presente; arquivo gerado não inspecionado |
| Upload | Artefato associado à execução contendo o arquivo | Action e `path` presentes; publicação não confirmada |
| Conteúdo | Texto literal `Test` | Inferência do comando; não é saída capturada da soma |

`test.txt` não contém resultado `3`, assertion, relatório ou quantidade de testes aprovados. Sua existência não comprova correção da soma nem segurança da aplicação.

## Como complementar com evidência de execução

Conferir numa execução existente: resultado do step `File`, conclusão do upload, artefato associado ao run e conteúdo do arquivo baixado. Registrar URL, commit, data e resultado observado. A disponibilidade depende também da retenção. [GitHub — upload-artifact](https://github.com/actions/upload-artifact).

Se a action Soma falhar, os steps seguintes normalmente não prosseguem nas condições padrão. Ausência de artefato exige examinar os logs anteriores. [GitHub — sintaxe de workflows](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax).

## Aprendizado e limites

O código liga geração de arquivo a upload. Para funcionar como evidência de teste, o conteúdo precisa vir do teste e permitir interpretar o resultado. Registrar entrada, saída e comparação com o esperado é uma evolução possível, ainda não implementada.

Não foi executado workflow, criado `test.txt` ou baixado artefato nesta documentação. O acesso remoto não foi confirmado; a análise usa código e histórico local.

## Evidências pendentes

- [ ] Vincular run com resultado do upload e identificação do artefato.
- [ ] Conferir arquivo baixado e registrar o conteúdo efetivo.
- [ ] Caso seja implementado relatório de testes, documentar sua estrutura e critério de aprovação.

## Referências e conexões

Fontes GitHub citadas acima, consultadas em 2026-10-07 e registradas em [[Referências DevSecOps]].

[[Laboratório — GitHub Actions — action composta Soma]] · [[Laboratório — GitHub Actions — jobs e eventos]] · [[GitHub Actions — workflows, jobs e steps]]

[[Laboratório DevSecOps|Voltar ao laboratório]]
