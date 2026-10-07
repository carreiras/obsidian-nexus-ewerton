# Ferramentas de segurança na pipeline

- Criado em: 2026-10-07
- Última pesquisa: 2026-10-07
- Escopo: visão geral das ferramentas, seus objetos de análise e limitações; não inclui um roteiro de instalação.

## Finalidade

Combinar ferramentas que examinam objetos diferentes: código, dependências, infraestrutura declarada e aplicação em execução. Docker pode fornecer o ambiente de execução dessas ferramentas; não é, por si só, um scanner de segurança. Os conceitos centrais estão em [[Verificações de segurança — SAST, DAST e SCA]] e [[Docker — imagens, containers e Dockerfile]].

| Ferramenta | Objeto e finalidade | Limitação a considerar |
|---|---|---|
| Horusec | Análise estática de código e busca de segredos; combina análises para diferentes linguagens | Cobertura depende das ferramentas, regras, linguagem e configuração |
| OWASP Dependency-Check | SCA: detectar vulnerabilidades divulgadas em dependências | Identificação dos componentes, fontes e atualização dos dados exigem atenção |
| KICS | Analisar IaC para encontrar configurações inseguras e problemas de conformidade | Examina arquivos e regras; não comprova sozinho o estado real do ambiente |
| ZAP | DAST de aplicações web, incluindo APIs; análise automatizada e manual | Depende de aplicação acessível e da cobertura de rotas, autenticação e estados |

Fontes oficiais: [Horusec](https://github.com/ZupIT/horusec), [Dependency-Check](https://github.com/dependency-check/DependencyCheck), [KICS](https://github.com/Checkmarx/kics), [ZAP — introdução](https://www.zaproxy.org/getting-started/).

## Horusec — código e segredos

Horusec é uma ferramenta open source de análise estática. Integra ferramentas e regras para diferentes linguagens e oferece busca de segredos nos arquivos e no histórico Git. O projeto informa que Docker é necessário para utilizar todas as ferramentas integradas; desabilitar essa dependência reduz a capacidade de análise. [ZupIT — Horusec](https://github.com/ZupIT/horusec).

Nesse contexto, orquestração significa coordenação das análises, inclusive em containers; não significa que Horusec seja uma plataforma geral de orquestração de aplicações.

## Dependency-Check — dependências

OWASP Dependency-Check é uma ferramenta de SCA. Relaciona dependências a vulnerabilidades publicadas, utilizando fontes como a **NVD**, com suporte e requisitos que variam por ecossistema. O repositório atual está na organização `dependency-check`. [Projeto oficial](https://github.com/dependency-check/DependencyCheck).

### Atualização e desempenho em CI

Instalar a ferramenta numa imagem não elimina a necessidade de atualizar sua base. O projeto recomenda chave da API NVD e estratégia de cache em CI devido à lentidão e aos limites de requisições. Seu exemplo Docker persiste o diretório de dados fora do container. Uma rotina agendada pode atualizar esses dados, mas exige compatibilidade e controle de uso compartilhado; não é garantia automática de desempenho. [Dependency-Check — requisitos e uso com Docker](https://github.com/dependency-check/DependencyCheck).

Na consulta de 2026-10-07, o projeto informa atualização obrigatória para **12.1.0 ou superior** por mudanças de compatibilidade da API NVD. Isso é um requisito mínimo informado, não uma recomendação de fixar essa versão indefinidamente. As próximas práticas devem conferir a release, os requisitos e os analisadores efetivamente utilizados. [Avisos do projeto](https://github.com/dependency-check/DependencyCheck).

Conexão: [[GitHub Actions — agendamento com cron]].

## KICS — infraestrutura como código

KICS significa *Keeping Infrastructure as Code Secure*. É um projeto open source da Checkmarx, com regras chamadas queries para identificar configurações inseguras e questões de conformidade em IaC. [Checkmarx — KICS](https://github.com/Checkmarx/kics).

Para ver o código e a interpretação, consulte [[Verificações de segurança — SAST, DAST e SCA#Exemplo 1 — usuário root no Dockerfile|Exemplo 1 — usuário root no Dockerfile]] e [[Verificações de segurança — SAST, DAST e SCA#Exemplo 2 — bloqueio de acesso público de um bucket S3 em Terraform|Exemplo 2 — bucket S3 privado em Terraform]]. Cada exemplo apresenta contexto, problema, ajuste e o que verificar. A cobertura do scanner depende do formato, da query e da versão; não há execução dessas regras registrada nas notas.

## ZAP — aplicação em execução

Zed Attack Proxy (ZAP) é uma ferramenta gratuita e open source que oferece scanners automatizados e recursos de teste manual para aplicações web. DAST não se restringe à tela visual: pode interagir com endpoints e APIs. [ZAP — introdução](https://www.zaproxy.org/getting-started/), [guia oficial](https://www.zaproxy.org/docs/desktop/).

O projeto anunciou sua associação à Checkmarx em setembro de 2024 mantendo a ferramenta open source. O nome atual nos materiais oficiais é ZAP by Checkmarx. [ZAP — anúncio oficial](https://www.zaproxy.org/blog/2024-09-24-zap-has-joined-forces-with-checkmarx/).

Para uma futura prática, definir alvo de teste autorizado, aplicação acessível, autenticação e caminhos a explorar. Não assumir que uma lista de endpoints ou ausência de alertas representa cobertura completa.

## Integração e tratamento dos resultados

Roteiro de planejamento, a adaptar ao repositório:

1. Definir o objeto e o risco que se pretende avaliar.
2. Escolher ferramenta, versão e regras compatíveis.
3. Preparar runner, código, configuração e, para DAST, aplicação em execução.
4. Executar a análise e preservar o relatório.
5. Investigar os alertas e corrigir ou justificar o tratamento.
6. Definir quando os resultados devem bloquear a continuidade.

Executar um scanner não cria automaticamente um **security gate**. O workflow precisa tratar resultado, código de saída e política de bloqueio. A nota [[GitHub Actions — workflows, jobs e steps]] já explica dependências, condições e o efeito de `continue-on-error`. [GitHub — sintaxe](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax).

## Escolha de ferramentas e limites das comparações

Uma opção de planejamento é experimentar controles com ferramentas open source antes de avaliar soluções comerciais. Isso não constitui uma sequência obrigatória de maturidade. Sugestão de avaliação: cobertura necessária, qualidade dos alertas, manutenção, integração, custo operacional e suporte.

Não há estudo comparativo registrado nesta nota que comprove menos falsos positivos em ferramentas comerciais ou superioridade do KICS sobre Terrascan. Essas comparações exigem critérios e evidências no contexto avaliado. Contar mais alertas, isoladamente, não comprova maior eficácia.

## Revisão técnica e continuidade

2026-10-07 — Verificadas as funções e classificações de Horusec, Dependency-Check, KICS e ZAP nas fontes oficiais. KICS foi identificado como open source; separada a instalação do Dependency-Check da atualização de dados. Fontes citadas nas seções.

Os exemplos não foram executados. Não há auditoria de manutenção, benchmark ou validação de eficácia dos scanners registrada nesta nota. Para implementar uma pipeline, ainda é necessário definir instalação, comandos, relatórios e políticas de bloqueio no repositório de aplicação.

Fontes consultadas em 2026-10-07, registradas em [[Referências DevSecOps]].

[[Verificações de segurança — SAST, DAST e SCA]] · [[Docker — imagens, containers e Dockerfile]] · [[GitHub Actions — workflows, jobs e steps]]

[[Índice — DevSecOps|Voltar ao índice]]
