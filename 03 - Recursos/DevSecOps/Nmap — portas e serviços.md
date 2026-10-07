# Nmap — portas e serviços

- Criado em: 2026-09-15
- Última pesquisa: 2026-09-15

## Conceito
Nmap permite investigar portas e serviços de um alvo. Uma varredura descreve o que foi observado a partir de determinado ponto da rede; não comprova, por si só, a existência de uma vulnerabilidade. [Nmap — fundamentos de varredura](https://nmap.org/book/man-port-scanning-basics.html).

## Exemplo ilustrativo
```sh
nmap 127.0.0.1
```

O endereço `127.0.0.1` é o loopback: o comando examina a própria máquina onde é executado, não toda a rede local.

Por padrão, esse comando verifica 1.000 portas TCP; não representa uma avaliação de todas as portas e protocolos. [Nmap — fundamentos de varredura](https://nmap.org/book/man-port-scanning-basics.html).

## Como ler o resultado
| Campo ou estado | Interpretação |
|---|---|
| `PORT`, como `80/tcp` | Número da porta e protocolo de transporte |
| `STATE` | Estado observado pelo scanner |
| `open` | Há uma aplicação aceitando conexões nessa porta TCP |
| `closed` | O alvo responde, mas não há aplicação escutando nessa porta |
| `filtered` | A filtragem impede determinar se a porta está aberta |
| `SERVICE` | Nome associado ao serviço; no comando básico, pode ser inferido pelo número da porta |

Há outros estados possíveis. A tabela destaca apenas os necessários para a leitura inicial. [Estados de portas](https://nmap.org/book/man-port-scanning-basics.html), [identificação de serviços](https://nmap.org/book/man-version-detection.html).

## Identificação de serviços
Um rótulo como `http` não comprova qual aplicação está em execução. Serviços podem usar portas diferentes das convencionais. A opção `-sV` faz sondagens adicionais para tentar identificar protocolo, produto e versão; a identificação ainda pode ser incompleta. [Nmap — detecção de serviço e versão](https://nmap.org/book/man-version-detection.html).

## Limites da conclusão
- Uma porta aberta pode atender a uma necessidade legítima; é preciso avaliar o serviço e sua configuração.
- O resultado no loopback não comprova acessibilidade pela internet. Interfaces, regras de rede e o ponto de observação influenciam o resultado.
- Uma lista de portas descreve o alvo e o momento da varredura; não deve ser atribuída a outro ambiente.

Os estados refletem a perspectiva da varredura, conforme o [manual do Nmap](https://nmap.org/book/man-port-scanning-basics.html).

## Aplicação ao hardening
Usar o levantamento para comparar serviços observados com serviços necessários e investigar exposições inesperadas. Relacionar os resultados a [[Hardening e operação segura]]. O comando é um exemplo ilustrativo, sem execução registrada nesta nota.

## Revisão técnica
2026-09-15 — Corrigido o alvo para `127.0.0.1`; distinguidos loopback, rede local e exposição pública. Separadas descoberta de portas, identificação de serviços e confirmação de vulnerabilidades.

## Referências
- [Nmap — Port Scanning Basics](https://nmap.org/book/man-port-scanning-basics.html) — consultado em 2026-09-15.
- [Nmap — Service and Version Detection](https://nmap.org/book/man-version-detection.html) — consultado em 2026-09-15.

[[Índice — DevSecOps|Voltar ao índice]]
