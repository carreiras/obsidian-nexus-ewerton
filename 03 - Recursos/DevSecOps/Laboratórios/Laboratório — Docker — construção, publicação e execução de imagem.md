# Laboratório — Docker — construção, publicação e execução de imagem

- Registrado em: 2026-10-07.
- Situação: construção, execução e publicação documentadas em registro pessoal; execução no GitHub Actions não confirmada por logs.

## Pergunta ou hipótese

Como empacotar um programa Python, executá-lo localmente, publicá-lo no Docker Hub e configurar sua execução no GitHub Actions? Pergunta formulada retrospectivamente pela IA a partir da prática relatada; não é uma expectativa pessoal previamente registrada.

## Conceitos e referências

[[Docker — imagens, containers e Dockerfile#Exemplo completo — programa Python em uma imagem]] explica os comandos e suas diferenças. [[GitHub Actions — workflows, jobs e steps#Executar uma imagem publicada no runner]] distingue execução no runner e implantação. Fontes técnicas oficiais consultadas em 2026-10-07: [Dockerfile](https://docs.docker.com/reference/dockerfile/), [run](https://docs.docker.com/reference/cli/docker/container/run/), [push](https://docs.docker.com/reference/cli/docker/image/push/) e [tags/digests](https://docs.docker.com/reference/cli/docker/image/pull/).

## Ambiente

- Projeto local: `C:\projetos\estudos\devsecops-with-github-action-docker`, contendo `Dockerfile` e `hi.py`; sem histórico Git identificado nessa pasta.
- Host: caminhos Windows e comandos apresentados como PowerShell. Versões do Windows, Docker Engine/Desktop, backend e arquitetura não informadas.
- Base declarada: `python:3.12`; versão efetiva do interpretador não coletada. O digest da base está abreviado no registro e não permite identificá-la precisamente.
- Imagem publicada no relato: `docker.io/ewertoncarreira/hello-docker:latest`.
- Workflow: checkout `C:\projetos\estudos\devsecops-with-github-actions`, snapshot `516504bccefaa9008748d5461f48c788a652a3dc`. Runner declarado `ubuntu-latest`; ambiente efetivo não confirmado.

## Passos executados

Sequência conforme o registro pessoal `C:\Users\ewert\Downloads\docker-build-passo-a-passo.md`, seguida da alteração comprovada no histórico Git. A ordem apresentada não reconstrói horários de execução nem tentativas não registradas.

1. Preparados os arquivos encontrados no projeto:

```dockerfile
FROM python:3.12
WORKDIR /app
COPY hi.py .
CMD ["python", "hi.py"]
```

```python
print("Hi :)")
```

2. Construída a imagem no diretório do projeto:

```powershell
docker build -t ewertoncarreira/hello-docker .
```

O registro resume `FINISHED`, 9/9 etapas e 1,4 s, com `WORKDIR` e `COPY` em cache. É um resumo pessoal da saída, não o log bruto completo.

3. Executado o programa na imagem:

```powershell
docker run ewertoncarreira/hello-docker
```

Saída registrada: `Hi :)`.

4. Autenticado e publicado no Docker Hub:

```powershell
docker login
docker push ewertoncarreira/hello-docker
```

O registro informa `Login Succeeded`, camadas `Pushed` e:

```text
latest: digest: sha256:68d4a02d0ddf3301b6fd2aff4efb0f63c67e39cebc458cccf91f6e60d676779c size: 856
```

5. Executado novamente com terminal e entrada aberta:

```powershell
docker run -it ewertoncarreira/hello-docker
```

Saída registrada: `Hi :)`. O programa não lê entrada e termina; `-it` não altera seu comando padrão.

6. Alterado o step do job `deploy` de `echo "Deploy"` para `docker run ewertoncarreira/hello-docker`, mantendo `needs: test`, no [commit 5960b914](https://github.com/carreiras/devsecops-with-github-actions/commit/5960b91465a11c4bcaa653ee1926a2cbfd359344). O commit seguinte, `516504bc`, atualiza o README. O diff local comprova a configuração; não comprova a execução remota.

**Exemplo separado da sequência comprovada:** o registro também apresenta `docker run --rm -it ewertoncarreira/hello-docker bash` e um prompt `root@a1b2c3d4e5f6:/app#`. O ID tem aparência ilustrativa e não há log independente para confirmar essa sessão. Portanto, `ls`, `pwd`, `python hi.py` e `exit` desse trecho ficam como demonstração, não como operações comprovadas. A ausência de `USER` no Dockerfile também não substitui uma verificação do usuário efetivo.

## Resultado esperado e observado

Expectativas abaixo inferidas pela IA a partir do código e da configuração.

| Etapa | Esperado | Observado no material |
|---|---|---|
| Build | Criar imagem com Python e script | Registro pessoal informa conclusão e cache |
| Execução local | Imprimir `Hi :)` e terminar | Mensagem registrada; código de saída e estado final não coletados |
| Publicação | Enviar a referência ao registry | Saída pessoal informa sucesso e digest; estado remoto atual não consultado |
| Execução com `-it` | Manter o mesmo programa padrão | Mesma mensagem registrada |
| GitHub Actions | Executar imagem depois de `test`, conforme condições do workflow | Comando presente no diff; resultado remoto pendente |

## Evidências, erros e soluções

Fontes: arquivos do projeto Docker, registro pessoal em Downloads e histórico Git local do projeto Actions. O arquivo de Downloads permanece preservado no local original; comandos, resultados essenciais e digest foram registrados aqui para permitir consulta sem depender dele.

Não há erro ou correção de erro documentado nessa prática. A entrada `[auth] pull token` do resumo de build não comprova por si só login na conta do Docker Hub; a evidência de login é a saída do comando `docker login`. A etapa `load .dockerignore` não comprova que um arquivo com exclusões tenha sido criado: nenhum `.dockerignore` foi encontrado na pasta analisada.

O acesso ao repositório remoto não foi confirmado nesta análise. Links de commits identificam as alterações verificadas localmente, sem atestar o estado atual do GitHub.

## Conclusões e limitações

Análise da IA: a prática conecta construção, execução local e distribuição de uma imagem. Os resultados pessoais registrados sustentam a mensagem impressa e a publicação naquele momento. No workflow, o job chamado `deploy` executa um programa no runner; não configura um serviço persistente nem constrói/publica a imagem. O job `build` continua apenas imprimindo `Build`.

O comando sem tag utiliza `latest`, referência mutável, e a política padrão de `run` baixa apenas se a imagem estiver ausente. O digest registrado identifica o conteúdo da publicação relatada, mas não há evidência de que uma execução remota tenha usado exatamente esse conteúdo. [Docker — execução](https://docs.docker.com/reference/cli/docker/container/run/), [digests](https://docs.docker.com/reference/cli/docker/image/pull/).

Não foram executados Docker ou workflows durante esta documentação. Permanecem sem confirmação as versões efetivas, arquitetura, usuário efetivo, códigos de saída, acessibilidade atual da imagem e logs do Actions. Nenhuma conclusão pessoal além do relato foi atribuída ao usuário.

[[Laboratório — GitHub Actions — jobs e eventos#Evolução — execução de imagem em 2026-10-07]] · [[Referências DevSecOps]]

[[Laboratório DevSecOps|Voltar ao laboratório]]
