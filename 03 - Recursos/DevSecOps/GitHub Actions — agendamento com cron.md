# GitHub Actions — agendamento com cron

- Criado em: 2026-10-02
- Última pesquisa: 2026-10-02
- Escopo: GitHub.com; exemplos didáticos, sem agendamento executado.

## Pergunta central

Como executar tarefas periódicas com GitHub Actions e quais são seus limites?

## Conceito

O evento `schedule` aciona um workflow em horários definidos por uma expressão cron POSIX. Pode atender a scans periódicos, relatórios ou outras tarefas com duração limitada. O job continua precisando de um runner; usar um runner hospedado pelo GitHub evita administrar uma máquina própria para essa execução. [GitHub — Schedule](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule), [runners hospedados](https://docs.github.com/en/actions/concepts/runners/github-hosted-runners).

## Campos do cron

```text
minuto  hora  dia-do-mês  mês  dia-da-semana
   *      *       *       *        *
```

| Campo | Valores |
|---|---|
| Minuto | 0–59 |
| Hora | 0–23 |
| Dia do mês | 1–31 |
| Mês | 1–12 |
| Dia da semana | 0–6; domingo é 0 |

`*` aceita qualquer valor; `,` lista valores; `-` define intervalo; `/` define incrementos. No YAML, a expressão fica entre aspas. Não acrescentar comando ou campo de segundos: o comando pertence aos steps. Atalhos como `@reboot` e `@daily` não são suportados. [GitHub — Schedule](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule).

| Expressão | Interpretação no fuso configurado |
|---|---|
| `'0 * * * *'` | A cada hora, no minuto zero |
| `'17 * * * *'` | A cada hora, no minuto 17 |
| `'0 0 * * *'` | Todos os dias à meia-noite |
| `'30 9 * * 1-5'` | Segunda a sexta às 9h30 |

A expressão `'0 */1 * * *'` equivale a executar a cada hora, no minuto zero. Evitar o início da hora pode reduzir a chance de atraso em períodos de carga elevada. [GitHub — Schedule](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule).

## Exemplo com fuso horário e acionamento manual

```yaml
name: Tarefa periódica de exemplo
on:
  schedule:
    - cron: '17 9 * * 1-5'
      timezone: America/Sao_Paulo
  workflow_dispatch:

permissions: {}

jobs:
  tarefa:
    runs-on: ubuntu-latest
    steps:
      - name: Demonstrar execução
        shell: bash
        run: echo 'Executando tarefa periódica de exemplo.'
```

O exemplo agenda dias úteis às 9h17 no fuso de São Paulo e permite execução manual. Sem `timezone`, o padrão é UTC. A documentação atual permite um identificador IANA de fuso horário; essa capacidade deve ser conferida separadamente se o ambiente for GitHub Enterprise Server. [GitHub — Schedule](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule).

## Limitações e aplicação

- O menor intervalo suportado é de cinco minutos.
- O workflow agendado precisa existir na branch padrão e executa sobre o commit mais recente dessa branch.
- Sob carga elevada, execuções podem atrasar e jobs enfileirados podem ser descartados.
- Em repositórios públicos, agendamentos são desativados após 60 dias sem atividade no repositório.

Essas regras são documentadas em [GitHub — Schedule](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule). Não há garantia de execução pontual.

Recomendação de aplicação: usar para tarefas que toleram essas condições. Atualizações de banco devem considerar repetição segura, falhas e execuções sobrepostas; tarefas que exigem horário rigoroso ou disponibilidade contínua precisam de uma solução que atenda a esses requisitos. É uma orientação derivada dos limites documentados, não um experimento realizado.

O agendamento também está sujeito a recursos, duração e limites do GitHub Actions. Pode exigir secrets, acesso à rede ou autorização para serviços externos. [GitHub — Limites](https://docs.github.com/en/actions/reference/limits), [[GitHub Actions — variáveis, secrets e autenticação]].

## Revisão técnica

2026-10-02 — Esclarecidos cron, schedule e a distinção entre tarefa agendada e servidor permanente. Acrescentados fuso horário, branch padrão e limites de execução conforme documentação atual. Atalhos do cron do Linux não devem ser copiados para GitHub Actions sem conferir a sintaxe suportada.

Nenhuma tarefa de banco, scan ou mensagem para Slack foi executada nesta etapa.

## Referências e conexões

Fontes: documentação oficial do GitHub citada em cada seção, consultada em 2026-10-02 e registrada em [[Referências DevSecOps]].

[[GitHub Actions — workflows, jobs e steps]] · [[GitHub Actions — variáveis, secrets e autenticação]] · [[Verificações de segurança — SAST, DAST e SCA]] · [[Laboratório DevSecOps]]

[[Índice — DevSecOps|Voltar ao índice]]
