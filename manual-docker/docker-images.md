# Docker Images

<a id='dokerfile-menu'></a>
## Principais instruções do Dockerfile

[FROM](#from) -  define a imagem base para a construção da imagem Docker.

[USER](#USER) - especifica qual usuário será utilizado para executar as operações seguintes na imagem.

[COPY](#COPY) - copia arquivos ou diretórios do contexto do build para o sistema de arquivos da imagem.

[WORKDIR](#WORKDIR) - define o diretório de trabalho dentro da imagem para as instruções subsequentes.

[ADD](#ADD) - copia arquivos ou diretórios para a imagem, com funcionalidades extras como descompactar arquivos e aceitar URLs.

[RUN](#RUN) - executa comandos durante o processo de build da imagem, geralmente para instalar pacotes ou configurar o ambiente.

[ENV](#ENV) - define variáveis de ambiente que estarão disponíveis na imagem e nos containers criados a partir dela.

[EXPOSE](#EXPOSE) - documenta a porta que a aplicação utilizará, informando quais portas serão expostas pelo container.

[CMD](#CMD) - especifica o comando padrão a ser executado quando um container criado a partir da imagem é iniciado.

##### Exemplos comentados de Dockerfile com otimização.

[Dockerfile com usuáro root]() - Utiliza o usuário root para executar todas as instruções. Embora seja mais simples de construir, essa abordagem não é recomendada por questões de segurança, pois o container roda com privilégios elevados, aumentando os riscos em caso de exploração.

[Dockerfile usuáro não root]() - Cria e utiliza um usuário com privilégios limitados para executar apenas as operações necessárias para a aplicação. Essa prática melhora a segurança do sistema, pois evita a exposição do usuário root e restringe as permissões do ambiente dentro do container.


## Descrição das principais instruções do Dockerfile

<a id='from'></a>

## FROM
#### Indica qual vai ser a plataforma/sistema operacional
###### Ex: node no linux alpine = node:12-alpine
FROM node:12-alpine

[retornar para - Instruções Dockerfile](#dokerfile-menu)
<a id='USER'></a>

## USER
##### Criando um grupo e um usuário...
##### ... e adicionando esse usuário ao grupo
RUN addgroup dev && adduser -S -G rprojetos dev
##### Setando o usuário que será utilizado na imagem
USER rprojetos

[retornar para - Instruções Dockerfile](#dokerfile-menu)
<a id='COPY'></a>

## COPY
#### Cenários de uso:
COPY file-1.pdf
COPY file-1.pdf file-2.pdf
COPY file-1.pdf /app
COPY file-1.pdf /app/
COPY file-1.pdf file-2.pdf /app/
COPY *.pdf /app/
COPY . /app/

[retornar para - Instruções Dockerfile](#dokerfile-menu)
<a id='WORKDIR'></a>

## WORKDIR
WORKDIR /app
COPY . .

[retornar para - Instruções Dockerfile](#dokerfile-menu)
<a id='ADD'></a>

## ADD
ADD https://um-site.com/arquivo.txt .
###### ADD descompacta arquivo para dentro do /app
ADD arquivo.zip .

[retornar para - Instruções Dockerfile](#dokerfile-menu)
<a id='RUN'></a>

## RUN
###### O comando RUN executa durante a construção da imagem
#### Instalação de dependencias da aplicação
###### obs: no S.O.(linux alpine), o camando de instalação é com apk e  não apt no caso do debian
RUN apk add --no-cache python2 g++ make
RUN yarn install --production

[retornar para - Instruções Dockerfile](#dokerfile-menu)
<a id='ENV'></a>

## ENV
#### definindo variáveis de ambiente
ENV API_URL=https://minha.api.com/
ENV API_KEY=123ABC456DEF789GHI

[retornar para - Instruções Dockerfile](#dokerfile-menu)
<a id='EXPOSE'></a>

## EXPOSE
### porta do container
###### para o caso do container no local host
###### https://localhost:3000
###### nesse caso o acesso da porta 3000 do container é possível
EXPOSE 3000

[retornar para - Instruções Dockerfile](#dokerfile-menu)
<a id='CMD'></a>

## CMD
###### O comando CMD executa depois que a imagem está contruida ao subir o container.
CMD [ "node", "./src/index.js" ]

[retornar para - Instruções Dockerfile](#dokerfile-menu)


## Exemplos:
### Dockerfile com usuário root [não recomendado]

```dockerfile
# plataforma : sistema-operacional
FROM node:12-alpine
# Define o diretório de trabalho
WORKDIR /app
# Instala dependências do sistema
RUN apk add --no-cache python2 g++ make
# Instala depêndencias do projeto
# Otimização: Copiando o arquivos de depências e 
# realizando a instalação dessas criamos um layer que
# entra em cache, e essa instalação somente rodaria
# novamente se houver mudança nas depências
COPY package.json .
RUN yarn install --production
# Copia os arquivos da aplicação para o container
COPY . . 
# Expõe a porta da aplicação
EXPOSE 3000
# Define o comando de inicialização
CMD [ "node", "./src/index.js" ]
```

### Dockerfile com usuário não root [recomendado]

```dockerfile
# Utiliza a imagem base oficial do Node.js 12 baseada no Alpine Linux para garantir uma imagem leve e otimizada
# plataforma : sistema-operacional
FROM node:12-alpine
# Cria o grupo e o usuário 'rprojetos'
RUN addgroup -S rprojetos && adduser -S -G rprojetos rprojetos
# Define o diretório de trabalho
WORKDIR /home/rprojetos/app
USER root
# Ajusta as permissões
RUN chown -R rprojetos:rprojetos /home/rprojetos/app
# Instala dependências do sistema
RUN apk add --no-cache python2 g++ make
# Instala depêndencias do projeto
# Otimização: Copiando o arquivos de depências e 
# realizando a instalação dessas criamos um layer que
# entra em cache, e essa instalação somente rodaria
# novamente se houver mudança nas depências
COPY package.json .
RUN yarn install --production
# Retorna para o usuário não-root
USER rprojetos
# Copia os arquivos da aplicação para o container
COPY . .
# Expõe a porta da aplicação
EXPOSE 3000
# Define o comando de inicialização
CMD [ "node", "./src/index.js" ]
```

<hr>

## Principais Comandos para manipulação imagens com DOCKER
1. > **docker build -t appweblistdocker .**
2. > **docker build --no-cache -t appweblistdocker .**
3. > **docker build -t appweblistdocker:v1.0.0 .**
4. > **docker run -it appweblistdocker sh**
5. > **docker run -p 3000:3000 appweblistdocker**
6. > **docker run -dp 3000:3000 appweblistdocker**
7. > **docker logs appweblistdocker**
8. > **docker image prune -f**
ou
**docker rmi $(docker images -f "dangling=true" -q)**
9. > **docker image remove appweblistdocker:v1.0.0**
10. > **docker image tag appweblistdocker:latest appweblistdocker:v1.0.0**
11. > **`docker ps`** 
12. > **`docker ps -a`** 
13. > **`docker stop <container_id|container_name>`** 
14. > **`docker rm <container_id|container_name>`** 
15. > **`docker image tag 9bcd793bb263 rprojetosit/appweblistdocker:v1.0.0`**


## Descrição dos principais Comandos para manipulação imagens com DOCKER

1. **`docker build -t appweblistdocker .`**  
    - **Descrição:** Constrói uma imagem Docker a partir do Dockerfile no diretório atual e a nomeia (tag) como `appweblistdocker`.  
    - **Uso:** Utiliza as camadas de cache se disponíveis para acelerar o build.

2. **`docker build --no-cache -t appweblistdocker .`**  
    - **Descrição:** Constrói a imagem Docker do mesmo jeito, mas sem utilizar nenhuma camada de cache, forçando a reconstrução completa.  
    - **Uso:** Útil quando você quer garantir que todas as etapas sejam executadas do zero.

3. **`docker build -t appweblistdocker:v1.0.0 .`**  
    - **Descrição:** Constrói uma imagem Docker a partir do Dockerfile no diretório atual e a nomeia (tag) como `appweblistdocker` com a versão v1.0.0.  
    - **Uso:** Utiliza as camadas de cache se disponíveis para acelerar o build.

4. **`docker run -it appweblistdocker sh`**  
    - **Descrição:** Executa um container interativamente a partir da imagem `appweblistdocker`, abrindo um terminal (`sh`) dentro do container.  
    - **Uso:** Permite acessar o shell do container para testes ou depuração.

5. **`docker run -p 3000:3000 appweblistdocker`**  
    - **Descrição:** Inicia um container da imagem `appweblistdocker` e mapeia a porta 3000 do container para a porta 3000 do host.  
    - **Uso:** Permite acessar a aplicação que roda no container através da porta 3000 do host.

6. **`docker run -dp 3000:3000 appweblistdocker`**  
    - **Descrição:** Inicia o container em modo *detached* (em segundo plano) e faz o mapeamento da porta 3000 do host para a porta 3000 do container.  
    - **Uso:** Ideal para rodar a aplicação sem ocupar o terminal.

7. **`docker logs appweblistdocker`**  
    - **Descrição:** Exibe os logs do container identificado como `appweblistdocker`.  
    - **Uso:** Útil para verificar a saída e depurar problemas na execução do container.

8. **`docker image prune -f`**
ou
**`docker rmi $(docker images -f "dangling=true" -q)`**
    - **Descrição:** remove as imagens "dangling" (aquelas que aparecem com `<none>` no repositório
    - **Uso:**  remover as imagens sem tag (dangling images) do seu sistema.

9. **`docker image remove appweblistdocker:v1.0.0`**  
    - **Descrição:** Remove a imagem Docker nomeada `appweblistdocker` com a tag `v1.0.0` do ambiente local.  
    - **Uso:** Útil para liberar espaço em disco ou remover versões obsoletas da imagem.

10. **`docker image tag appweblistdocker:latest appweblistdocker:v1.0.0`**  
    - **Descrição:** Cria uma nova tag `v1.0.0` para a imagem `appweblistdocker` que atualmente possui a tag `latest`.  
    - **Uso:** Permite versionar uma imagem existente sem recriá-la, facilitando a identificação e o gerenciamento de versões.

11. **`docker ps`**  
    - **Descrição:** Lista todos os containers que estão em execução no momento.  
    - **Uso:** Permite visualizar os containers ativos, com informações como container ID, imagem, status, portas e outros detalhes.

12. **`docker ps -a`**  
    - **Descrição:** Lista todos os containers, incluindo aqueles que estão parados ou finalizados.  
    - **Uso:** Útil para inspecionar o histórico de containers criados e para depuração, permitindo identificar containers que não estão mais em execução.

13. **`docker stop <container_id|container_name>`**  
    - **Descrição:** Interrompe a execução de um container em execução, enviando um sinal SIGTERM e permitindo que o container finalize de forma graciosa.  
    - **Uso:** Substitua `<container_id|container_name>` pelo ID ou nome do container que deseja parar.  
    - **Exemplo:**  
      ```bash
      docker stop 2fad168ecb3e
      ```  
      ou  
      ```bash
      docker stop appweblistdocker
      ```

14. **`docker rm <container_id|container_name>`**  
    - **Descrição:** Remove um container parado, identificando-o pelo ID ou nome.  
    - **Uso:** Após parar o container com `docker stop`, o comando `docker rm` remove o container, liberando recursos e mantendo o ambiente limpo.  
    - **Exemplo:**  
      ```bash
      docker rm 2fad168ecb3e
      ```  
      ou  
      ```bash
      docker rm app_container
      ```

15. **`docker image tag 9bcd793bb263 rprojetosit/appweblistdocker:v1.0.0`**  
    - **Descrição:** Associa uma nova tag à imagem Docker identificada pelo ID `9bcd793bb263`.  
    - **Uso:** Este comando renomeia a imagem para `rprojetosit/appweblistdocker` e atribui a ela a tag `v1.0.0`, permitindo que a imagem seja referenciada por esse nome e versão. Isso é útil para versionamento ou para facilitar o envio (push) da imagem a um registro (registry).


## Compartilhamento de imagens
1. Logar em https://hub.docker.com 
2. Criar um novo repositório Ex.: rprojetosit/appweblistdocker
3. Logar a partir do terminal de comando. 
    > **docker login**
4. Pegar o IMAGE ID executando o comando:
    Obs.: O IMAGE ID é referente a imagem que será enviada.
    > **docker images**
5. Associar uma nova tag à imagem Docker identificada pelo IMAGE ID `9bcd793bb263`.
    > **docker image tag 9bcd793bb263  rprojetosit/appweblistdocker:v1.0.0**

6. Envia (push) da imagem `rprojetosit/appweblistdocker:v1.0.0` a um registro (registry), no caso aqui para o https://hub.docker.com
    > **`docker push rprojetosit/appweblistdocker:v1.0.0`**

## Salvando e carregando imagens
1. Salvando a imagem `appweblistdocker:v1.0.0` em um arquivo tar chamado `appweblistdocker-v1.0.0.tar`
    > **`docker image save -o appweblistdocker-v1.0.0.tar appweblistdocker:v1.0.0`**

2. Carrega a imagem contida no arquivo `appweblistdocker-v1.0.0.tar` para o ambiente Docker.
    > **`docker image load -i appweblistdocker-v1.0.0.tar`**

