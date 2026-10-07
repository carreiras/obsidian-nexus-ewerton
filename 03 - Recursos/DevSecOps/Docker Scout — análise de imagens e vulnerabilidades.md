# Docker Scout — análise de imagens e vulnerabilidades

- Criado e pesquisado em: 2026-10-07.
- Escopo: análise de componentes de imagens, triagem e critérios de bloqueio; exemplos não executados.

## Para que serve

Docker Scout inventaria componentes de um artefato em uma **SBOM** (*Software Bill of Materials*, lista de componentes) e relaciona esse inventário a informações de vulnerabilidades. A imagem contém mais que o código da aplicação: inclui componentes herdados da base e dependências adicionadas no build. Scout pode ser usado pela CLI, Docker Hub e plataforma web. [Docker — visão geral do Scout](https://docs.docker.com/scout/).

A análise de vulnerabilidades de componentes se relaciona a [[Verificações de segurança — SAST, DAST e SCA|SCA]]. Ela não testa endpoints como DAST, nem comprova que uma CVE seja explorável no contexto da aplicação. Uma SBOM é um inventário, não um certificado de segurança.

## Preparação

O Docker Desktop inclui a CLI do Scout; outros ambientes podem instalar o plugin separadamente. Consultar a instalação oficial para o sistema utilizado e conferir `docker scout version`. Entrar na conta Docker com `docker login` ou pelo Docker Desktop, conforme a configuração inicial oficial. Para acompanhamento de repositórios remotos na plataforma, configurar também sua habilitação no Scout; acesso ao registry e acompanhamento contínuo são aspectos diferentes. Recursos e limites dependem da modalidade de uso. [Instalação](https://docs.docker.com/scout/install/), [configuração inicial](https://docs.docker.com/scout/quickstart/).

Os exemplos pressupõem CLI disponível, acesso aos serviços necessários e permissão para analisar a imagem. O nome `ewertoncarreira/hello-docker:latest` remete a uma imagem previamente publicada; sua disponibilidade atual não foi confirmada. Para uma análise reproduzível, preferir digest e registrar plataforma, versão da CLI e data.

## Exemplo — analisar uma imagem publicada

No PowerShell:

```powershell
$imagem = "registry://ewertoncarreira/hello-docker:latest"
docker scout quickview $imagem
docker scout cves --format sarif --output scout-cves.sarif $imagem
docker scout recommendations $imagem
```

`registry://` exige resolução no registry; `local://` selecionaria a imagem no armazenamento local. Essa distinção evita confundir uma imagem local antiga com a referência publicada. `cves` gera um relatório SARIF para inspeção ou integração posterior; gerar o arquivo não o publica automaticamente no GitHub. [Referência de cves](https://docs.docker.com/reference/cli/docker/scout/cves/).

**Como ler os resultados:** `quickview` resume vulnerabilidades e, quando disponíveis, informações da base. `recommendations` sugere atualização ou substituição da base; não altera o Dockerfile nem valida compatibilidade. Verificar o componente afetado antes de decidir uma mudança. [Quickview](https://docs.docker.com/reference/cli/docker/scout/quickview/), [recommendations](https://docs.docker.com/reference/cli/docker/scout/recommendations/).

**Resultado esperado:** resumo, relatório e recomendações quando houver informação suficiente. Não se conhece antecipadamente a quantidade de alertas. Se ocorrer erro de autenticação, acesso ou análise, não interpretar a ausência de relatório como resultado sem vulnerabilidades.

## Triagem e correção

Para cada alerta, conferir identificador, componente, versão instalada, versão corrigida quando informada, origem na imagem e exposição na aplicação. Severidade ajuda a priorizar, mas não substitui a análise de impacto. A seção [[Verificações de segurança — SAST, DAST e SCA#Triagem de alertas]] distingue falso positivo de aceitação de risco.

**Exemplo conceitual:** um pacote herdado de `FROM python:3.12` apresenta um alerta e existe base compatível com correção. Avaliar a recomendação, atualizar a referência apropriada e reconstruir a imagem, depois executar testes e novo scan. Alterar o script `hi.py` sozinho não atualiza o pacote herdado da base. Verificar o digest do novo artefato; a tag anterior pode apontar para outro conteúdo. [[Docker — imagens, containers e Dockerfile]] explica construção e identidade de imagens.

Para comparar duas versões já existentes no registry:

```powershell
docker scout compare registry://SEU_USUARIO/minha-app:candidata --to registry://SEU_USUARIO/minha-app:anterior
```

Substituir `SEU_USUARIO` e ambas as referências por imagens realmente publicadas. `--to` identifica a referência de comparação. Examinar componentes e vulnerabilidades adicionados ou removidos, sem concluir segurança apenas pela redução do total. O comando é documentado como **experimental**; conferir comportamento na versão instalada. [Compare](https://docs.docker.com/reference/cli/docker/scout/compare/).

## Exemplo — bloquear por severidade

**Política ilustrativa:** falhar se forem encontrados alertas de severidade alta ou crítica. No PowerShell:

```powershell
docker scout cves --only-severity critical,high --exit-code registry://ewertoncarreira/hello-docker:latest
$codigoScout = $LASTEXITCODE
if ($codigoScout -eq 2) {
    Write-Output "Bloqueado pela política de vulnerabilidades."
} elseif ($codigoScout -ne 0) {
    Write-Output "A análise falhou; conferir os logs."
}
```

`--exit-code` faz `cves` retornar **2** quando detecta vulnerabilidades segundo a análise filtrada. Consultar `$LASTEXITCODE` imediatamente evita perder o resultado para outro programa. Este trecho interpreta o código; num script usado como gate, terminar com `exit $codigoScout` propaga o bloqueio ao processo chamador. [Referência de cves](https://docs.docker.com/reference/cli/docker/scout/cves/).

No GitHub Actions, um step pode executar o mesmo comando diretamente:

```yaml
- name: Verificar vulnerabilidades da imagem
  shell: bash
  run: docker scout cves --only-severity critical,high --exit-code registry://ewertoncarreira/hello-docker:latest
```

**Fragmento de step**, não workflow completo: exige Scout instalado e autenticação preparada no runner. Não adicionar `continue-on-error` se esse resultado deve bloquear a sequência. Para implementação, a integração oficial oferece `docker/scout-action`; revisar inputs, versão e permissões antes de adotá-la. [Integração oficial](https://docs.docker.com/scout/integrations/ci/gha/), [[GitHub Actions — workflows, jobs e steps]].

**Limites da política:** alertas menores ficam fora do gate; isso não significa ausência de risco. Filtrar apenas CVEs corrigíveis pode ajudar a definir uma política, mas oculta problemas sem correção disponível. Registrar filtros, exceções justificadas e o artefato efetivamente analisado.

## Limites e conexões

Resultados dependem de componentes identificados, dados de vulnerabilidades, filtros e plataforma. Dados novos podem mudar o relatório de um mesmo digest. Não foram executados scans, testes de correção ou workflows nesta documentação; nenhuma saída ou CVE foi atribuída à imagem pessoal.

[[Trivy — vulnerabilidades, segredos e configurações inseguras]] · [[Ferramentas de segurança na pipeline]] · [[Referências DevSecOps]]

[[Índice — DevSecOps|Voltar ao índice]]
