# Laboratório — GitHub Actions — action composta Soma

- Registrado e pesquisado em: 2026-10-07
- Alterações analisadas: 2026-10-04 a 2026-10-06
- Situação: implementação e correções identificadas; saída de execução não confirmada.

## Pergunta do experimento

Como criar uma action reutilizável que receba dois valores e execute um script Python localizado no diretório da própria action?

## Ambiente e evidências

Projeto pessoal de Ewerton: [devsecops-with-github-actions](https://github.com/carreiras/devsecops-with-github-actions). Checkout local: `C:\projetos\estudos\devsecops-with-github-actions`; snapshot `55fe6ed4fb8095ba4d82e0caacdb5dce7583a2e6`. Arquivos: `action.yaml`, `soma.py` e `.github/workflows/main.yaml`.

A action declara Python `3.10`, `actions/checkout@v2` e `actions/setup-python@v4`. São referências encontradas, não recomendações de versões atuais. As versões efetivamente resolvidas não foram verificadas.

## Passos executados

Sequência das alterações comprovadas pelo histórico Git local; não é um relato de execuções confirmadas:

1. **2026-10-04 — `c29a42d`:** substituído o `echo` do step `Test` por chamada à action `carreiras/devsecops-with-github-action@main`, inicialmente com `a` vindo de uma referência a secret e `b: 4`. O diff não revela o valor do secret nem comprova que ele estivesse cadastrado.
2. **2026-10-04 — `e076791`:** corrigido o nome do repositório chamado para `carreiras/devsecops-with-github-actions@main`.
3. **2026-10-06 — `47ae841`:** criado `action.yaml` com `runs.using: composite`, inputs obrigatórios, checkout, preparação de Python 3.10 e chamada ao script.
4. **No mesmo commit:** criado `soma.py`, recebendo `--a` e `--b`, convertendo os valores com `int` e imprimindo a soma. No workflow, substituídos os inputs anteriores por constantes `a: 1` e `b: 2`.
5. **2026-10-06 — `707aed8`:** alterado o comando de `python soma.py -a=... -b=...` para o caminho da própria action com `github.action_path` e argumentos `--a`/`--b`, conforme as opções declaradas no script.

**Não reconstruído:** mensagens dos erros encontrados durante as tentativas, comandos executados localmente, versão da action resolvida no GitHub e saída após a correção. Os commits comprovam a implementação e os ajustes; não comprovam sucesso de execução.

## Implementação identificada

Em `action.yaml`, `runs.using: composite` agrupa os steps; os inputs `a` e `b` são obrigatórios. O workflow chama:

```yaml
- name: Test
  uses: carreiras/devsecops-with-github-actions@main
  with:
    a: 1
    b: 2
```

O step Python da action utiliza:

```yaml
- name: "run Soma"
  shell: bash
  run: python ${{ github.action_path }}/soma.py --a=${{ inputs.a }} --b=${{ inputs.b }}
```

`github.action_path` identifica o diretório da action composta, evitando depender do caminho relativo ao repositório consumidor. Os trechos são fragmentos, não um workflow completo. [GitHub — metadados de actions](https://docs.github.com/en/actions/reference/workflows-and-actions/metadata-syntax).

Em `soma.py`, `argparse` recebe `--a` e `--b` como strings. O cálculo ocorre depois com `int(a) + int(b)` e é impresso com `print(soma)`. A conversão não ocorre no parser; entradas não convertíveis geram erro em `int`. [Python 3.10 — argparse](https://docs.python.org/3.10/library/argparse.html).

## Correções comprovadas

| Data | Commit | Mudança e significado |
|---|---|---|
| 2026-10-04 | [c29a42d](https://github.com/carreiras/devsecops-with-github-actions/commit/c29a42d43977ebffec44c9f126d754055be56ae9) | Substitui o echo pela chamada de action, antes da inclusão local de `action.yaml` |
| 2026-10-04 | [e076791](https://github.com/carreiras/devsecops-with-github-actions/commit/e07679189f0b8b7c1e82f79cb6a447a0c65e132e) | Corrige referência de `github-action` para `github-actions`: o nome precisa corresponder ao repositório |
| 2026-10-06 | [47ae841](https://github.com/carreiras/devsecops-with-github-actions/commit/47ae8411f5194004b59c2167b83e43c18d2247f9) | Acrescenta `action.yaml` e `soma.py` |
| 2026-10-06 | [707aed8](https://github.com/carreiras/devsecops-with-github-actions/commit/707aed8493837abe6fd1d3fc84d17a8de15d8b6c) | Substitui `python soma.py -a=... -b=...` pelo caminho com `github.action_path` e flags `--a`/`--b` |

O histórico comprova ajustes. Sem logs anteriores e posteriores, não se atribui uma mensagem de erro específica nem sucesso após a correção.

## Resultado esperado e observado

- Esperado: com `a: 1` e `b: 2`, imprimir `3`, se preparação e execução terminarem normalmente.
- Observado: inputs, conversão e impressão implementados no snapshot local. A saída `3` não foi observada numa execução nesta análise.
- Limitação: não há assertion comparando a saída com `3`, nem output declarado na action. Executar a soma no job `test` não verifica automaticamente o valor produzido.

## Aprendizado e pontos a avaliar

O código conecta `with`, inputs, argumentos de linha de comando e cálculo Python. A correção do caminho mostra por que uma action reutilizável precisa localizar seus próprios arquivos.

Para evoluir, avaliar dois pontos: `@main` pode resolver uma versão diferente do commit do workflow; inputs interpolados diretamente no texto Bash exigem cuidado se vierem de fontes não confiáveis. Atualmente são constantes `1` e `2`. Permissões mínimas, referências imutáveis e passagem por variáveis de ambiente com aspas são medidas a avaliar; nenhuma foi implementada nesta documentação. [GitHub — uso seguro](https://docs.github.com/en/actions/reference/security/secure-use).

## Evidências pendentes

- [ ] Vincular execução mostrando versão da action, Python e saída do script.
- [ ] Se forem realizados testes, registrar casos positivos, negativos e entrada inválida, com resultados reais.
- [ ] Documentar futura assertion ou output quando implementado e verificado.

O projeto não foi alterado nem executado. GitHub remoto e logs não foram confirmados; a evidência é o código e o histórico local.

## Referências e conexões

Fontes GitHub e Python citadas acima, consultadas em 2026-10-07 e registradas em [[Referências DevSecOps]].

[[GitHub Actions — workflows, jobs e steps]] · [[GitHub Actions — variáveis, secrets e autenticação]] · [[Laboratório — GitHub Actions — jobs e eventos]]

[[Laboratório DevSecOps|Voltar ao laboratório]]
