# Trivy — vulnerabilidades, segredos e configurações inseguras

- Criado e pesquisado em: 2026-10-07.
- Escopo: CLI, imagens e arquivos locais, interpretação de alertas e gates; exemplos não executados.

## Para que serve

Trivy é um scanner open source da Aqua Security. Possui alvos como imagens, sistemas de arquivos e repositórios, com scanners para vulnerabilidades de componentes, segredos, configurações inseguras e licenças. Recursos disponíveis dependem do alvo e da configuração. [Projeto oficial](https://github.com/aquasecurity/trivy).

Não chamar todas essas análises de SAST: dependências vulneráveis se relacionam a SCA; arquivos Terraform ou Dockerfile exigem análise de configuração; busca de credenciais é análise de segredos. Trivy não executa um teste DAST de endpoints nesses comandos. [[Verificações de segurança — SAST, DAST e SCA]] explica a distinção.

Na análise de imagens, vulnerabilidades e segredos são habilitados por padrão na documentação consultada. Informar `--scanners` torna o escopo explícito; `--scanners vuln` exclui busca de segredos nesse comando. [Seleção de scanners](https://trivy.dev/docs/latest/configuration/others/).

## Preparação

No Windows, a instalação oficial orienta baixar o ZIP da release para Windows, extrair e disponibilizar o executável. Os exemplos pressupõem `trivy` no `PATH`; se estiver apenas no diretório atual, usar `./trivy.exe`. Conferir `trivy --version` e registrar a versão utilizada. Linux e execução em container possuem instruções próprias. [Instalação oficial](https://trivy.dev/docs/latest/getting-started/installation/).

Não é necessário instalar Docker para consultar uma imagem exclusivamente no registry com o scanner nativo; é necessário acesso ao registry e aos dados do scanner. Preparar autenticação para imagens privadas. Os exemplos não instalam nem executam ferramentas durante a documentação.

## Exemplo — vulnerabilidades de uma imagem

No PowerShell:

```powershell
$imagem = "ewertoncarreira/hello-docker:latest"
trivy image --image-src remote --scanners vuln $imagem
trivy image --image-src remote --scanners vuln --format json --output trivy-image.json $imagem
```

`--image-src remote` seleciona o registry. Sem seleção explícita, Trivy pode procurar primeiro em Docker, Containerd e Podman locais antes do registry. Isso pode produzir análise de conteúdo diferente sob a mesma tag. Para repetir a análise do mesmo artefato, substituir a tag por digest e manter a plataforma comparável. [Alvo imagem](https://trivy.dev/docs/latest/guide/target/container_image/).

`--scanners vuln` delimita a análise a vulnerabilidades. O primeiro comando mostra resultados no terminal; o segundo grava JSON. O relatório pode mudar entre execuções conforme os dados atualizados. **Resultado esperado:** identificação de componentes e alertas quando encontrados; não há número de vulnerabilidades conhecido para essa imagem. [Formatos de relatório](https://trivy.dev/docs/latest/configuration/reporting/).

## Como interpretar um alerta

| Campo | Pergunta que ajuda a responder |
|---|---|
| Componente e versão instalada | Qual pacote está realmente presente? |
| Identificador da vulnerabilidade | Qual aviso precisa ser investigado? |
| Severidade | Qual impacto técnico é indicado pela fonte? |
| Versão corrigida | Existe atualização informada para esse componente? |
| Alvo e localização | O componente pertence ao sistema operacional ou à aplicação? |

Trivy utiliza fontes de advisories conforme o ecossistema e pode priorizar informações do fornecedor da distribuição, inclusive correções por backport. Não concluir vulnerabilidade apenas por comparar números de versão com outra distribuição. [Scanner de vulnerabilidades](https://trivy.dev/docs/latest/guide/scanner/vulnerability/).

**Exemplo conceitual:** um pacote da base Linux aparece no relatório do programa Python. A correção pode exigir atualizar e reconstruir a base, não editar o `print` do aplicativo. Após a mudança, testar o programa e analisar a nova imagem. Se não há versão corrigida, avaliar mitigação ou exceção justificada; não classificar automaticamente como falso positivo.

## Exemplo — configuração e segredos em arquivos

No diretório de um projeto autorizado, usar análises separadas para entender os objetos:

```powershell
trivy config .
trivy fs --scanners secret .
```

O primeiro examina arquivos de configuração reconhecidos, como Dockerfile e Terraform. Ler regra, arquivo, trecho e recomendação antes de alterar. Por exemplo, ao investigar usuário root, conferir também a configuração de execução; a nota [[Verificações de segurança — SAST, DAST e SCA#Exemplo 1 — usuário root no Dockerfile]] desenvolve problema, ajuste e verificação. A detecção depende das regras e formatos suportados. [Scanner de configuração](https://trivy.dev/docs/latest/guide/scanner/misconfiguration/).

O segundo procura padrões de segredos nos arquivos selecionados. **Resultado esperado:** alertas com localização se houver correspondências; não há detecção conhecida para o seu projeto. Se uma credencial real foi exposta, removê-la do arquivo não basta: revogar ou rotacionar e investigar uso indevido. Não publicar relatórios de segredos sem avaliar o conteúdo sensível. Esse comando não equivale a examinar todo o histórico Git. [Scanner de segredos](https://trivy.dev/docs/latest/guide/scanner/secret/).

## Exemplo — gate e relatório

**Política ilustrativa:** bloquear por vulnerabilidades altas ou críticas, incluindo as sem correção disponível.

```powershell
trivy image --image-src remote --scanners vuln --severity HIGH,CRITICAL --exit-code 1 --format json --output trivy-gate.json ewertoncarreira/hello-docker:latest
$codigoTrivy = $LASTEXITCODE
if ($codigoTrivy -ne 0) {
    Write-Output "Análise bloqueada ou com erro; conferir relatório e logs."
}
```

Por padrão, resultados encontrados não tornam a execução uma falha. `--exit-code 1` muda isso para os alertas selecionados. Erros operacionais também podem causar falha: verificar logs antes de tratar todo código não zero como vulnerabilidade. Em script chamado pela CI, acrescentar `exit $codigoTrivy` ao final propaga o resultado. [Scanners e códigos de saída](https://trivy.dev/docs/latest/configuration/others/).

Fragmento de step no GitHub Actions, supondo CLI já instalada no runner:

```yaml
- name: Verificar vulnerabilidades da imagem
  shell: bash
  run: trivy image --image-src remote --scanners vuln --severity HIGH,CRITICAL --exit-code 1 --format json --output trivy-gate.json ewertoncarreira/hello-docker:latest
```

Esse fragmento não instala Trivy, configura credenciais nem publica o JSON. Preservar o relatório em step posterior com condição `if: always()` permite coletá-lo mesmo após bloqueio; não utilizar `continue-on-error` se o scan deve impedir a continuidade. Um workflow completo exige definir também a versão do scanner, permissões, retenção e dependências. [[GitHub Actions — workflows, jobs e steps]] e [[Laboratório — GitHub Actions — publicação de artefatos]] explicam execução e upload.

`--ignore-unfixed` excluiria vulnerabilidades sem correção conhecida; não corrige nem elimina sua exposição. Um sistema sem suporte pode ter cobertura insuficiente: conferir avisos e considerar política explícita para fim de vida, em vez de celebrar zero alertas. [Opções e fim de vida](https://trivy.dev/docs/latest/configuration/others/).

## Dados, cache e limites

Trivy utiliza base de vulnerabilidades, dados Java quando aplicáveis e bundles de checks. Cache reduz downloads, mas exige atualização e compatibilidade com a versão usada. `--skip-db-update` não cria uma base ausente nem garante análise atual. Registrar versão do scanner, digest, plataforma, data, filtros e identificação dos dados disponíveis. [Bases e atualização](https://trivy.dev/docs/latest/configuration/db/).

Zero alertas significa que nada foi encontrado dentro da cobertura efetiva, não que a aplicação seja segura. Scanners diferentes podem identificar pacotes e classificar problemas de formas distintas; comparar o mesmo artefato e contexto. Nenhum scan, correção ou integração foi executado nesta documentação.

[[Docker Scout — análise de imagens e vulnerabilidades]] · [[Docker — imagens, containers e Dockerfile]] · [[Ferramentas de segurança na pipeline]] · [[Referências DevSecOps]]

[[Índice — DevSecOps|Voltar ao índice]]
