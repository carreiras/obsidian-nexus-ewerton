# GitHub Actions — variáveis, secrets e autenticação

- Criado em: 2026-10-02
- Última pesquisa: 2026-10-02
- Escopo: GitHub.com; exemplos didáticos, sem autenticação real realizada.

## Pergunta central

Como fornecer configuração e credenciais a um workflow sem gravar segredos no código?

## Configuração e informação sensível

| Recurso | Finalidade | Referência |
|---|---|---|
| `env` | Variável de ambiente definida no workflow, job ou step | No Bash: `$NOME`; em expressões: `${{ env.NOME }}` |
| `vars` | Configuração não sensível cadastrada no GitHub | `${{ vars.NOME }}` |
| `secrets` | Valores sensíveis cadastrados no GitHub | `${{ secrets.NOME }}` |

`env` é um mecanismo de passagem de valores; não torna uma senha protegida. Um secret pode ser fornecido como variável de ambiente ao processo que precisa dele. Configurações com `vars` não devem conter credenciais. [GitHub — Variáveis](https://docs.github.com/en/actions/concepts/workflows-and-actions/variables), [uso de secrets](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets).

## Escopos e passagem ao shell

`env` pode valer para o workflow, para um job ou para um step. Durante a execução, uma definição mais específica prevalece sobre uma definição mais ampla com o mesmo nome. Secrets podem ser cadastrados no repositório, organização ou environment, conforme acesso e recursos disponíveis. Environment é um contexto de implantação, com configurações e possíveis regras de proteção. [GitHub — Sintaxe de env](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax#env), [secrets](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets).

Trecho didático para um job Ubuntu; pressupõe o secret `API_TOKEN` previamente cadastrado:

```yaml
steps:
  - name: Conferir disponibilidade da credencial
    shell: bash
    env:
      API_TOKEN: ${{ secrets.API_TOKEN }}
    run: |
      if [ -z "$API_TOKEN" ]; then
        echo 'Credencial não disponível.'
        exit 1
      fi
      echo 'Credencial disponível; valor não exibido.'
```

Esse trecho verifica apenas presença; não valida a credencial nem faz uma chamada autenticada. `${{ secrets.API_TOKEN }}` resolve o valor pelo contexto do GitHub, e `$API_TOKEN` é lido pelo Bash. A sintaxe do shell muda entre Bash e PowerShell. [GitHub — Variáveis](https://docs.github.com/en/actions/concepts/workflows-and-actions/variables).

## Cuidados com acesso e exposição

- Fornecer o segredo apenas aos jobs e steps que precisam dele e usar credenciais com privilégios mínimos.
- Evitar imprimir valores sensíveis. A ocultação automática nos logs não cobre necessariamente valores transformados.
- Revisar actions e scripts que recebem segredos: o código executado pode acessar os valores disponíveis.
- Revogar ou rotacionar credenciais expostas; apagar um trecho de log ou código não invalida uma credencial.
- Definir permissões mínimas para o `GITHUB_TOKEN`, aumentando somente quando a tarefa exigir.

Essas recomendações derivam de [GitHub — Uso seguro](https://docs.github.com/en/actions/reference/security/secure-use). Secrets armazenados não significam que “mais ninguém” poderá acessá-los por meio de um workflow autorizado.

Em workflows originados de forks, secrets normalmente não são fornecidos, com exceção do `GITHUB_TOKEN`, sujeito às regras do evento e às configurações. Workflows acionados pelo Dependabot também têm restrições. Não contornar essas proteções executando código não confiável com credenciais privilegiadas. [GitHub — Uso de secrets](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets), [uso seguro](https://docs.github.com/en/actions/reference/security/secure-use).

## Autenticação em nuvem com OIDC

OIDC permite que um provedor de nuvem confie na identidade de um job e emita credenciais temporárias, evitando armazenar uma chave de longa duração como secret. Exige configuração de confiança no provedor, restrições de identidade e permissões compatíveis com a tarefa. [GitHub — OpenID Connect](https://docs.github.com/en/actions/concepts/security/openid-connect).

No job que solicita o token OIDC, `permissions: id-token: write` autoriza sua obtenção. Isso não concede, por si só, acesso a recursos da nuvem: a política de confiança e as permissões do provedor determinam o acesso. Para AWS, avaliar o papel IAM e limitar quais repositórios, branches ou environments podem assumi-lo. [GitHub — OIDC com AWS](https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-aws).

## Revisão técnica

2026-10-02 — Separados `env`, `vars` e `secrets`; corrigida a ideia de que uma variável de ambiente ou secret elimina qualquer possibilidade de exposição. Acrescentada a alternativa OIDC para nuvem. Fontes oficiais citadas nas seções.

Os exemplos não comprovam uma integração funcional com AWS. Não há criação, uso ou teste de credenciais registrado nesta nota.

## Referências e conexões

Fontes: documentação oficial do GitHub citada em cada seção, consultada em 2026-10-02 e registrada em [[Referências DevSecOps]].

[[GitHub Actions — workflows, jobs e steps]] · [[Hardening e operação segura]] · [[Requisitos de segurança]] · [[Laboratório DevSecOps]]

[[Índice — DevSecOps|Voltar ao índice]]
