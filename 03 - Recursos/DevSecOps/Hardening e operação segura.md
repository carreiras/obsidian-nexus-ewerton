# Hardening e operação segura

- Criado em: 2026-09-11
- Última pesquisa: 2026-09-11

## Conceito
Hardening é o fortalecimento da configuração para reduzir exposição e privilégios desnecessários. Infraestrutura como código ajuda a versionar, revisar e verificar essas configurações. [OWASP — Segurança de infraestrutura como código](https://cheatsheetseries.owasp.org/cheatsheets/Infrastructure_as_Code_Security_Cheat_Sheet.html).

## Práticas a avaliar
- Restringir acessos e permissões ao necessário.
- Remover serviços e funcionalidades dispensáveis.
- Proteger segredos e evitar sua inclusão em código.
- Revisar configurações de rede e exposição pública.
- Verificar a infraestrutura antes da implantação e acompanhar desvios depois. [OWASP — Segurança de infraestrutura como código](https://cheatsheetseries.owasp.org/cheatsheets/Infrastructure_as_Code_Security_Cheat_Sheet.html).

Exemplo ilustrativo: em uma aplicação web, o banco não precisa estar acessível diretamente pela internet apenas porque a API atende usuários externos. O acesso deve refletir os fluxos necessários.

## Transporte seguro
Usar TLS e validar sua configuração, incluindo certificados e protocolos aceitos. Atualizar bibliotecas criptográficas e desabilitar protocolos inseguros faz parte da manutenção. [OWASP — TLS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html).

Mudar uma porta ou ocultar um banner não demonstra que um serviço esteja protegido. Essas medidas não substituem restrição de acesso, autenticação e correção de vulnerabilidades.

Para interpretar um levantamento de portas e seus limites, consulte [[Nmap — portas e serviços]].

## Operação
Planejar monitoramento e resposta, acompanhar falhas e encaminhar os resultados às equipes responsáveis pela correção. A implantação não encerra o trabalho de segurança. [NIST NCCoE — Modelo de referência DevSecOps](https://pages.nist.gov/nccoe-devsecops/notational-reference-model.html).

## Revisão técnica
2026-09-11 — Esclarecido o conceito de hardening. Evitada a orientação simplista de instalar sempre a versão mais nova: avaliar suporte, correções e compatibilidade do ambiente. Pentest e hardening não são exclusivos da etapa de deploy.

[[Ciclo de desenvolvimento de software seguro — SSDLC]] · [[Verificações de segurança — SAST, DAST e SCA]]

## Referências
- [OWASP — Segurança de infraestrutura como código](https://cheatsheetseries.owasp.org/cheatsheets/Infrastructure_as_Code_Security_Cheat_Sheet.html) — consultado em 2026-09-11.
- [OWASP — TLS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html) — consultado em 2026-09-11.
- [NIST NCCoE — Modelo de referência DevSecOps](https://pages.nist.gov/nccoe-devsecops/notational-reference-model.html) — consultado em 2026-09-11.

[[Índice — DevSecOps|Voltar ao índice]]
