# Requisitos de segurança

- Criado em: 2026-09-11
- Última pesquisa: 2026-09-15 (complemento sobre OWASP; demais referências mantêm suas datas)

## Conceito
Requisitos de segurança descrevem propriedades ou comportamentos que precisam ser satisfeitos e verificados. Podem ser derivados de riscos, políticas, padrões e histórico de vulnerabilidades. [OWASP — Requisitos de segurança](https://devguide.owasp.org/en/03-requirements/).

“Não funcional” não significa opcional: a segurança pode envolver requisitos funcionais e não funcionais. O OWASP recomenda o ASVS como apoio à definição de requisitos; o Top 10 é uma referência de riscos e conscientização, não uma especificação completa. [OWASP — Programa de segurança de aplicações (Top 10:2025)](https://owasp.org/Top10/2025/0x03_2025-Establishing_a_Modern_Application_Security_Program/).

## Edições e escopos do OWASP Top 10
Na consulta de 2026-09-15, a edição publicada mais recente do Top 10 web é **2025**. A edição 2021 é histórica; ao citar categorias ou posições, informar a edição. [OWASP — Top 10 web](https://owasp.org/www-project-top-ten/).

O OWASP Top 10 web aborda riscos de aplicações web. Existem projetos específicos para [API Security](https://owasp.org/www-project-api-security/) e [Mobile Top 10](https://owasp.org/www-project-mobile-top-10/). Escolher a referência conforme o contexto da aplicação, sem tratar as listas como equivalentes.

## Exemplo ilustrativo: acesso a pedidos
| Necessidade | Requisito proposto | Verificação |
|---|---|---|
| Restringir pedidos ao proprietário | A API deve negar leitura de pedido de outra conta | Testar duas contas e tentativa de acesso cruzado |
| Proteger contas administrativas | Exigir MFA no acesso administrativo | Verificar que a etapa adicional é exigida |
| Evitar exposição em erros | Não retornar segredos nem detalhes internos na resposta | Inspecionar respostas de falha |
| Proteger o transporte | Usar HTTPS com TLS configurado adequadamente | Verificar protocolo, certificado e redirecionamentos |

São exemplos para orientar a escrita, não uma lista completa nem controles já implementados.

## Proteções diferentes
- **TLS** protege a comunicação; SSL é uma denominação antiga e seus protocolos não devem ser adotados. A orientação OWASP prioriza TLS 1.3, permitindo TLS 1.2 quando necessário. [OWASP — TLS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html).
- **Hash de senha** é usado para verificar senhas sem armazená-las de forma reversível. A OWASP recomenda funções apropriadas, como Argon2id, com salt e parâmetros de custo adequados. Hash não é sinônimo de criptografia reversível. [OWASP — Armazenamento de senhas](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html).
- **MFA** trata a autenticação com múltiplos fatores; não equivale à criptografia do banco.

## Aplicação
Para cada requisito, registrar risco tratado, escopo e evidência esperada. Conectar o requisito ao teste e à decisão de design que o implementa. [OWASP — Requisitos de segurança](https://devguide.owasp.org/en/03-requirements/).

## Revisão técnica
2026-09-15 — Atualizada a indicação da edição vigente do Top 10 web e esclarecidos os projetos específicos para APIs e mobile, conforme fontes OWASP abaixo.

2026-09-11 — Corrigida a classificação de segurança como sempre funcional; separados MFA, TLS e armazenamento de senhas. Referências abaixo sustentam as distinções.

[[Ciclo de desenvolvimento de software seguro — SSDLC]] · [[Modelagem de ameaças e STRIDE]]

## Referências
- [OWASP — Top 10 web](https://owasp.org/www-project-top-ten/) — consultado em 2026-09-15.
- [OWASP — API Security](https://owasp.org/www-project-api-security/) — consultado em 2026-09-15.
- [OWASP — Mobile Top 10](https://owasp.org/www-project-mobile-top-10/) — consultado em 2026-09-15.
- [OWASP — Requisitos de segurança](https://devguide.owasp.org/en/03-requirements/) — consultado em 2026-09-11.
- [OWASP — Programa de segurança de aplicações (Top 10:2025)](https://owasp.org/Top10/2025/0x03_2025-Establishing_a_Modern_Application_Security_Program/) — consultado em 2026-09-11.
- [OWASP — TLS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html) — consultado em 2026-09-11.
- [OWASP — Armazenamento de senhas](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) — consultado em 2026-09-11.

[[Índice — DevSecOps|Voltar ao índice]]

