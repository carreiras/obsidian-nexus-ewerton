# Referências DevSecOps

As notas citam as referências junto ao conteúdo correspondente.

## Consultadas em 2026-09-11

- [NIST NCCoE — Modelo de referência DevSecOps](https://pages.nist.gov/nccoe-devsecops/notational-reference-model.html).
- [OWASP — Programa de segurança de aplicações (Top 10:2025)](https://owasp.org/Top10/2025/0x03_2025-Establishing_a_Modern_Application_Security_Program/).
- [Microsoft — STRIDE](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats).
- [OWASP — Dependency-Check](https://owasp.org/www-project-dependency-check/).
- [OWASP — Modelagem de ameaças](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html).
- [OWASP — Segurança de infraestrutura como código](https://cheatsheetseries.owasp.org/cheatsheets/Infrastructure_as_Code_Security_Cheat_Sheet.html).
- [OWASP — TLS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html).
- [OWASP — Armazenamento de senhas](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html).
- [OWASP — Requisitos de segurança](https://devguide.owasp.org/en/03-requirements/).
- [OWASP — Developer Guide](https://owasp.org/www-project-developer-guide/assets/exports/OWASP_Developer_Guide.pdf).

## Consultadas em 2026-09-15
- [OWASP — Top 10 web](https://owasp.org/www-project-top-ten/) — consultado em 2026-09-15.
- [OWASP — API Security](https://owasp.org/www-project-api-security/) — consultado em 2026-09-15.
- [OWASP — Mobile Top 10](https://owasp.org/www-project-mobile-top-10/) — consultado em 2026-09-15.
- [Nmap — Port Scanning Basics](https://nmap.org/book/man-port-scanning-basics.html) — consultado em 2026-09-15.
- [Nmap — Service and Version Detection](https://nmap.org/book/man-version-detection.html) — consultado em 2026-09-15.

- [AWS — DevOps](https://aws.amazon.com/devops/what-is-devops/) — consultado em 2026-09-15.
- [AWS — DevSecOps](https://aws.amazon.com/what-is/devsecops/) — consultado em 2026-09-15.
- [AWS — Application Security](https://aws.amazon.com/what-is/application-security/) — consultado em 2026-09-15.
- [DORA — Continuous integration](https://dora.dev/capabilities/continuous-integration/) — consultado em 2026-09-15.
- [DORA — Continuous delivery](https://dora.dev/capabilities/continuous-delivery/) — consultado em 2026-09-15.
- [OWASP SAMM — Organization and Culture](https://owaspsamm.org/model/governance/education-and-guidance/stream-b/) — consultado em 2026-09-15.
- [GitHub — Triagem de alertas](https://docs.github.com/en/code-security/how-tos/manage-security-alerts/manage-code-scanning-alerts/triage-alerts-in-pull-requests) — consultado em 2026-09-15.
- [Cucumber — BDD](https://cucumber.io/docs/bdd/) — consultado em 2026-09-15.
- [Scrum Guide](https://scrumguides.org/scrum-guide.html) — consultado em 2026-09-15.

## Consultadas em 2026-10-02

Documentação oficial do GitHub para GitHub.com:

- [Workflows](https://docs.github.com/en/actions/concepts/workflows-and-actions/workflows).
- [Sintaxe de workflows](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax).
- [Eventos e schedule](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows).
- [Runners hospedados pelo GitHub](https://docs.github.com/en/actions/concepts/runners/github-hosted-runners).
- [Runners próprios](https://docs.github.com/en/actions/concepts/runners/self-hosted-runners).
- [Limites do GitHub Actions](https://docs.github.com/en/actions/reference/limits).
- [Actions personalizadas](https://docs.github.com/en/actions/concepts/workflows-and-actions/custom-actions).
- [Workflows reutilizáveis](https://docs.github.com/en/actions/concepts/workflows-and-actions/reusing-workflow-configurations).
- [Variáveis](https://docs.github.com/en/actions/concepts/workflows-and-actions/variables).
- [Uso de secrets](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets).
- [Uso seguro](https://docs.github.com/en/actions/reference/security/secure-use).
- [OpenID Connect](https://docs.github.com/en/actions/concepts/security/openid-connect).
- [OIDC com AWS](https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-aws).

## Consultadas em 2026-10-07

- [GitHub — eventos e schedule](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule) — reconsultada para o laboratório de jobs e eventos.
- [GitHub — metadados de actions](https://docs.github.com/en/actions/reference/workflows-and-actions/metadata-syntax) — actions compostas e localização de scripts.
- [GitHub — upload-artifact](https://github.com/actions/upload-artifact) — publicação e retenção de artefatos.
- [GitHub — uso seguro](https://docs.github.com/en/actions/reference/security/secure-use) — reconsultada para inputs e referências de actions.
- [Python 3.10 — argparse](https://docs.python.org/3.10/library/argparse.html).

### Evidências locais dos laboratórios

Atualização de 2026-10-07: evolução Docker do workflow consultada no snapshot local `516504bccefaa9008748d5461f48c788a652a3dc`, preservando o snapshot anterior nos registros históricos. Consultados também `Dockerfile` e `hi.py` em `C:\projetos\estudos\devsecops-with-github-action-docker` e o registro pessoal `C:\Users\ewert\Downloads\docker-build-passo-a-passo.md`. Saídas locais são evidências registradas pelo usuário; não foram reproduzidas nesta documentação.

- [Docker — execução de containers](https://docs.docker.com/reference/cli/docker/container/run/) — comando padrão, terminal, remoção e política de pull.
- [Docker — publicação de imagens](https://docs.docker.com/reference/cli/docker/image/push/).
- [Docker — obtenção de imagens, tags e digests](https://docs.docker.com/reference/cli/docker/image/pull/).
- [Docker — cache de construção](https://docs.docker.com/build/cache/).
- [GitHub — runners hospedados](https://docs.github.com/en/actions/concepts/runners/github-hosted-runners) — reconsultada para execução de imagem no runner.

Projeto pessoal: [carreiras/devsecops-with-github-actions](https://github.com/carreiras/devsecops-with-github-actions). Arquivos e histórico Git consultados no checkout `C:\projetos\estudos\devsecops-with-github-actions`, snapshot `55fe6ed4fb8095ba4d82e0caacdb5dce7583a2e6`. Os commits pertinentes estão vinculados em cada registro. A consulta remota não foi confirmada; os links não substituem a evidência local nem comprovam resultados de execução.

### Fontes dos conceitos e ferramentas

- [Docker — USER](https://docs.docker.com/reference/dockerfile/#user).
- [Node.js — boas práticas para imagens Docker](https://github.com/nodejs/docker-node/blob/main/docs/BestPractices.md).
- [npm — ci](https://docs.npmjs.com/cli/v11/commands/npm-ci/).
- [KICS — queries de Dockerfile](https://docs.kics.io/latest/queries/dockerfile-queries/).
- [KICS — queries de Terraform](https://docs.kics.io/latest/queries/terraform-queries/).
- [AWS — Block Public Access no S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html).
- [HashiCorp — s3_bucket_public_access_block](https://github.com/hashicorp/terraform-provider-aws/blob/main/website/docs/r/s3_bucket_public_access_block.html.markdown) — conteúdo consultado na documentação bruta oficial.

- [Docker — visão geral](https://docs.docker.com/get-started/docker-overview/).
- [Docker — visão geral do Dockerfile](https://docs.docker.com/build/concepts/dockerfile/).
- [Docker — referência do Dockerfile](https://docs.docker.com/reference/dockerfile/).
- [Docker — builds multiplataforma](https://docs.docker.com/build/building/multi-platform/).
- [Docker — boas práticas de construção](https://docs.docker.com/build/building/best-practices/).
- [Docker — publicação de portas](https://docs.docker.com/get-started/docker-concepts/running-containers/publishing-ports/).
- [ZupIT — Horusec](https://github.com/ZupIT/horusec).
- [OWASP — Dependency-Check, repositório atual](https://github.com/dependency-check/DependencyCheck).
- [Checkmarx — KICS](https://github.com/Checkmarx/kics).
- [ZAP — introdução](https://www.zaproxy.org/getting-started/).
- [ZAP — guia oficial](https://www.zaproxy.org/docs/desktop/).
- [ZAP — associação à Checkmarx, anúncio de 2024](https://www.zaproxy.org/blog/2024-09-24-zap-has-joined-forces-with-checkmarx/).
- [GitHub — sintaxe de workflows](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax) — reconsultada para tratamento de falhas e security gates.

## Notas relacionadas
- [[Laboratório — Docker — construção, publicação e execução de imagem]]
- [[Laboratório — GitHub Actions — jobs e eventos]]
- [[Laboratório — GitHub Actions — action composta Soma]]
- [[Laboratório — GitHub Actions — publicação de artefatos]]
- [[Docker — imagens, containers e Dockerfile]]
- [[Ferramentas de segurança na pipeline]]
- [[GitHub Actions — workflows, jobs e steps]]
- [[GitHub Actions — variáveis, secrets e autenticação]]
- [[GitHub Actions — agendamento com cron]]
- [[DevOps, DevSecOps e AppSec]]
- [[Integração, entrega e implantação contínuas]]
- [[Security Champions]]
- [[Ciclo de desenvolvimento de software seguro — SSDLC]]
- [[Requisitos de segurança]]
- [[Modelagem de ameaças e STRIDE]]
- [[Verificações de segurança — SAST, DAST e SCA]]
- [[Hardening e operação segura]]
- [[Nmap — portas e serviços]]

[[Índice — DevSecOps|Voltar ao índice]]


