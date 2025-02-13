#### <- [Retornar para DevOps](https://github.com/rprojetos/devops/blob/main/README.md)

# Docker Container

<a id='container-menu'></a>
## Principais Comandos para manipulação de containers com DOCKER:

[****1.Status do container****](#cmd-docker-1)

1.1 [**`docker ps`**](#cmd-docker-1.1)

1.2 [**`docker ps -a`**](#cmd-docker-1.2)


[****2.Criar e iniciar o container****](#cmd-docker-2)

2.1 [**`docker run -it appweblistdocker:v1.0.0 sh`**](#cmd-docker-2.1)

2.2 [**`docker run -p hostPort:containerPort appweblistdocker:v1.0.0`**](#cmd-docker-2.2)

2.3 [**`docker run -d -p hostPort:containerPort appweblistdocker:v1.0.0`**](#cmd-docker-2.3)

2.4 [**`docker run -d -p hostPort:containerPort --name appweblist-1 appweblistdocker:v1.0.0`**](#cmd-docker-2.4)


[****3.Acessar logs do container****](#cmd-docker-3)

3.1 [**`docker logs -f <container_id|container_name>`**](#cmd-docker-3.1)


[****4.Acessar o terminal do container****](#cmd-docker-4)

4.1 [**`docker exec <container_id|container_name> cmd`**](#cmd-docker-4.1)

4.2 [**`docker exec -it <container_id|container_name> sh`**](#cmd-docker-4.2)


[****5.Iniciar e parar containers existentes****](#cmd-docker-5)

5.1 [**`docker start <container_id|container_name>`**](#cmd-docker-5.1)

5.2 [**`docker stop <container_id|container_name>`**](#cmd-docker-5.2)


[****6.Remover containers****](#cmd-docker-6)

6.1 [**`docker rm <container_id|container_name>`**](#cmd-docker-6.1)

6.2 [**`docker rm -f <container_id|container_name>`**](#cmd-docker-6.2)


[****7.Cópia de arquivos entre host e container****](#cmd-docker-7)

7.1 [**`docker cp <source_path> <container_id|container_name>:<destination_path>`**](#cmd-docker-7.1)


[****8.Operações com Volume "host/container"****](#cmd-docker-8)

8.1 [**`docker volume create <nome_do_volume>`**](#cmd-docker-8.1)

8.2 [**`Criando um volume que utilize um caminho específico no host`**](#cmd-docker-8.2)

8.3 [**`docker volume ls`**](#cmd-docker-8.3)

8.4 [**`docker volume inspect <nome_do_volume>`**](#cmd-docker-8.4)

8.5 [**`Mapeamento de volume entre host e container`**](#cmd-docker-8.5)

## [Anotações Gerais](#notas)

[**Observações sobre volumes**](#notas-volumes)

[**Controle de acessos**](#notas-acessos)

[**Comandos uteis**](#notas-Comandos-uteis)

<br>

## Descrição dos principais Comandos para manipulação de containers com DOCKER

<hr>

<a id='cmd-docker-1'></a>
****1.Status do container****

<a id='cmd-docker-1.1'></a>
1.1 **`docker ps`**
    - **Descrição:** Lista todos os containers que estão em execução no momento.  
    - **Uso:** Permite visualizar os containers ativos, com informações como container ID, imagem, status, portas e outros detalhes.

[retornar para o menu](#container-menu)

<a id='cmd-docker-1.2'></a>
1.2 **`docker ps -a`**  
    - **Descrição:** Lista todos os containers, incluindo aqueles que estão parados ou finalizados.  
    - **Uso:** Útil para inspecionar o histórico de containers criados e para depuração, permitindo identificar containers que não estão mais em execução.

[retornar para o menu](#container-menu)

<hr>

<a id='cmd-docker-2'></a>
****2.Criar e iniciar o container****

<a id='cmd-docker-2.1'></a>
2.1 **`docker run -it appweblistdocker:v1.0.0 sh`**  
    - **Descrição:** Executa um container interativamente a partir da imagem `appweblistdocker:v1.0.0`, abrindo um terminal (`sh`) dentro do container.  
    - **Uso:** Permite acessar o shell do container para testes ou depuração.

[retornar para o menu](#container-menu)

<a id='cmd-docker-2.2'></a>
2.2 **`docker run -p hostPort:containerPort appweblistdocker:v1.0.0`**  
    - **Descrição:** Inicia um container da imagem `appweblistdocker:v1.0.0` e mapeia a porta 3000 do container para a porta 3000 do host.  
    - **Uso:** Permite acessar a aplicação que roda no container através da porta 3000 do host
    **Exemplo:**
        ##### mapeamento de portas host=3000 e container=3000
        > `docker run -p 3000:3000 appweblistdocker:v1.0.0`

[retornar para o menu](#container-menu)

<a id='cmd-docker-2.3'></a>
2.3 **`docker run -d -p hostPort:containerPort appweblistdocker:v1.0.0`**  
    - **Descrição:** Inicia o container em modo *detached* (em segundo plano) e faz o mapeamento da porta 3000 do host para a porta 3000 do container a partir da imagem `appweblistdocker:v1.0.0`  
    - **Uso:** Ideal para rodar a aplicação sem ocupar o terminal.
    **Exemplo:**
        ##### mapeamento de portas host=3000 e container=3000
        > `docker run -d -p 3000:3000 appweblistdocker:v1.0.0`

[retornar para o menu](#container-menu)

<a id='cmd-docker-2.4'></a>
2.4 **`docker run -d -p hostPort:containerPort --name appweblist-1 appweblistdocker:v1.0.0`**  
    - **Descrição:** Inicia o container em modo *detached* (em segundo plano), faz o mapeamento da porta 3000 do host para a porta 3000 do container e nomeia o container para `appweblist-1` a partir da imagem `appweblistdocker:v1.0.0`
    - **Uso:** Ideal para rodar a aplicação sem ocupar o terminal.
    **Exemplo:**
        ##### mapeamento de portas host=3000 e container=3000
        > `docker run -d -p 3000:3000 --name appweblist-1 appweblistdocker:v1.0.0`

[retornar para o menu](#container-menu)

<hr>

<a id='cmd-docker-3'></a>
****3.Acessar logs do container****

<a id='cmd-docker-3.1'></a>
3.1 **`docker logs -f <container_id|container_name>`**
    - **Flag -f:** log mostrando sempre os últimos outputs.
    - **Flag -n:** mostra o log de um determinado numeros de linha. Ex.: as ultimas 10 linhas.
    - **Flag -t:** mostra a data e horário do log
    - **Descrição:** Exibe os logs do container identificado como `appweblist-1`.  
    - **Uso:** Útil para verificar a saída e depurar problemas na execução do container.
    **Exemplos:**
        ##### visualizando todo o log
        > `docker logs appweblist-1`
        ##### visualizando os ultimos outputs
        > `docker logs -f appweblist-1`
        ##### visualizando apenas as 10 últimas linhas
        > `docker logs -n 10 appweblist-1`
        ##### visualizando o timestamp dos logs
        > `docker logs -t appweblist-1`

[retornar para o menu](#container-menu)

<hr>

<a id='cmd-docker-4'></a>
****4.Acessar o terminal do container****

<a id='cmd-docker-4.1'></a>
4.1 **`docker exec <container_id|container_name> cmd`**  
   - **Descrição:** Executa um comando específico dentro de um container que já está em execução. Isso permite interagir com o ambiente interno do container sem precisar acessá-lo de forma interativa.
   - **Uso:** É utilizado para realizar operações pontuais, depurar ou inspecionar o estado interno do container. Você pode executar comandos simples ou complexos conforme necessário.
   - **Exemplos:**  
     ```bash
     docker exec 2fad168ecb3e ls
     ```  
     ou  
     ```bash
     docker exec appweblist-1 pwd
     ```  

[retornar para o menu](#container-menu)

<a id='cmd-docker-4.2'></a>
4.2 **`docker exec -it <container_id|container_name> sh`**  
   - **Descrição:** Executa de forma interativa o shell `sh` dentro de um container que está em execução, permitindo acesso a um terminal interno para inspeção e depuração.  
   - **Uso:** Utilizado para abrir uma sessão de shell interativa dentro do container, possibilitando a execução de comandos manuais, a verificação de arquivos e a análise do ambiente interno. Os flags `-i` e `-t` garantem que o terminal seja interativo e que um pseudo-TTY seja alocado.  
   - **Exemplos:**  
     ```bash
     docker exec -it 2fad168ecb3e sh
     ```  
     ou  
     ```bash
     docker exec -it appweblist-1 sh
     ```  

[retornar para o menu](#container-menu)

<hr>

<a id='cmd-docker-5'></a>
****5.Iniciar e parar containers existentes****

<a id='cmd-docker-5.1'></a>
5.1 **`docker start <container_id|container_name>`**  
    - **Descrição:** Inicia um container que está parado, reativando sua execução a partir do estado em que foi interrompido.  
    - **Uso:** Utilizado quando um container previamente criado e parado precisa ser reiniciado sem precisar criar um novo container.  
    - **Exemplo:**  
      ```bash
      docker start appweblist-1
      ```
      ou
      ```bash
      docker start f8d601dac46f
      ```

[retornar para o menu](#container-menu)


<a id='cmd-docker-5.2'></a>
5.2. **`docker stop <container_id|container_name>`**  
    - **Descrição:** Interrompe a execução de um container em execução, enviando um sinal SIGTERM e permitindo que o container finalize de forma graciosa.  
    - **Uso:** Substitua `<container_id|container_name>` pelo ID ou nome do container que deseja parar.  
    - **Exemplo:**  
      ```bash
      docker stop 2fad168ecb3e
      ```  
      ou  
      ```bash
      docker stop appweblist-1
      ```

[retornar para o menu](#container-menu)

<hr>

<a id='cmd-docker-6'></a>
****6.Remover containers****

<a id='cmd-docker-6.1'></a>
6.1 **`docker rm <container_id|container_name>`**  
    - **Descrição:** Remove um container parado, identificando-o pelo ID ou nome.  
    - **Uso:** Após parar o container com `docker stop`, o comando `docker rm` remove o container, liberando recursos e mantendo o ambiente limpo.  
    - **Exemplo:**  
      ```bash
      docker rm 2fad168ecb3e
      ```  
      ou  
      ```bash
      docker rm app_container:v1.0.0
      ```

[retornar para o menu](#container-menu)

<a id='cmd-docker-6.2'></a>
6.2 **`docker rm -f <container_id|container_name>`**  
    - **Descrição:** Remove um container que esta rodando, (-f) de maneira forçada, identificando-o pelo ID ou nome.  
    - **Uso:** Remove o container usando modo forçado `-f`, liberando recursos e mantendo o ambiente limpo.  
    - **Exemplo:**  
      ```bash
      docker rm -f 2fad168ecb3e
      ```  
      ou  
      ```bash
      docker rm -f app_container:v1.0.0
      ```

[retornar para o menu](#container-menu)

<hr>

<a id='cmd-docker-7'></a>
****7.Cópia de arquivos entre host e container****

<a id='cmd-docker-7.1'></a>
7.1 **`docker cp <source_path> <container_id|container_name>:<destination_path>`**  
   - **Descrição:** Copia arquivos ou diretórios entre o sistema de arquivos do host e um container em execução (ou vice-versa).  
   - **Uso:** Utilizado para transferir dados para dentro ou para fora de um container. Ao especificar o destino, se o caminho estiver no container, deve ser precedido pelo identificador ou nome do container seguido de `:`.  
   - **Exemplos:**  
     - Copiando do host para o container:  
       ```bash
       docker cp ./test2.txt 0921363f0510:/home/rprojetos/app/
       ```  
     - Copiando do container para o host:  
       ```bash
       docker cp 0921363f0510:/home/rprojetos/app/test.txt .
       ```  

[retornar para o menu](#container-menu)

<hr>

<a id='cmd-docker-8'></a>
****8.Operações com Volume "host/container"****

<a id='cmd-docker-8.1'></a>
8.1 **`docker volume create <nome_do_volume>`**  
   - **Descrição:** Cria um volume Docker com o nome especificado.  
   - **Uso:** Utilizado para persistir dados e permitir que sejam compartilhados entre containers, mantendo os dados mesmo que os containers sejam recriados ou removidos.  
   - **Exemplos:**  
     ```bash
     docker volume create appweblist-1-dados
     ```

[retornar para o menu](#container-menu)
  
<a id='cmd-docker-8.2'></a>
8.2 Criando um volume que utilize um caminho específico no host utilizando o driver local com opções de bind mount.

    **Sintax:** `docker volume create --driver local --opt type=none --opt device=/caminho/especifico/no/host --opt o=bind meu_volume`
    **Exemplo**:
    ```bash
    docker run -d -p 3000:3000 --name appweblist-v1 -v vol_data:/home/rprojetos/app/dados appweblistdocker:v1.0.0
    ```

    [retornar para o menu](#container-menu)

<a id='cmd-docker-8.3'></a>
8.3 **`docker volume ls`**  
    - **Descrição:** Lista todos os volumes gerenciados pela Docker Engine.  
    - **Uso:** Exibe os volumes existentes no ambiente, mostrando informações como o nome do volume e o driver utilizado.  
    - **Exemplo:**  
      ```bash
      docker volume ls
      ```

[retornar para o menu](#container-menu)


<a id='cmd-docker-8.4'></a>
8.4 **`docker volume inspect <nome_do_volume>`**  
   - **Descrição:** Exibe informações detalhadas sobre o volume especificado, incluindo seu caminho de montagem, driver utilizado e outras configurações.  
   - **Uso:** Útil para diagnosticar e verificar as propriedades de um volume, garantindo que ele esteja configurado conforme o esperado.  
   - **Exemplos:**  
     ```bash
     docker volume inspect appweblist-1-dados
     ```

[retornar para o menu](#container-menu)


<a id='cmd-docker-8.5'></a>
8.5 **`docker run -d -p <hostPort:containerPort> --name <container> -v <volume-docker>:<mapeamento-volume-dentro-container> <IMAGE_ID|REPOSITORY:TAG>`**  
   - **Descrição:** Inicia um container em modo detached (segundo plano) a partir da imagem especificada. O comando mapeia uma porta do host para uma porta do container, define um nome para o container e monta um volume Docker no caminho desejado dentro do container, permitindo a persistência e o compartilhamento de dados.
   - **Uso:**  
     Utilizado para iniciar aplicações Docker que precisam estar acessíveis através de uma porta definida e que exigem persistência de dados. O mapeamento de portas (`-p`) possibilita o acesso à aplicação a partir do host, e o mapeamento de volume (`-v`) assegura que os dados gerados pela aplicação sejam preservados mesmo após a reinicialização ou remoção do container.
   - **Exemplo:**  
     ```bash
     docker run -d -p 3000:3000 --name appweblist-v1 -v appweblist-1-dados:/home/rprojetos/app/dados appweblistdocker:v1.0.0
     ```

[retornar para o menu](#container-menu)

<hr><hr>

<a id='notas'></a>
## Anotações Gerais

## [Anotações Gerais](#notas)

<hr>

<a id='notas-volumes'></a>
**Observações sobre volumes:**

Para containers que foram criados a partir de uma imagem que utiliza usuário `não root` temos que dar acesso total para esse usuário dentro do diretório correspondente `/home/user`, para que este tenha acesso ao volume que foi criado/mapeado.
1. Inicie uma nova sessão do terminal como root 
```docker exec -it -u root <container_id|container_name> sh```
2. Dê permissão total para o usuário
```bash
chown -R user:user /home/user
```
- **Exemplo:**  
    Acesso o terminal do container no modo interativo como root:
     ```bash
     docker exec -it -u root 0921363f0510 sh
     ```
     No terminal do container ex.: `~/app $` executar:
     ```bash
     chown -R rprojetos:rprojetos /home/rprojetos
     ```
     
[retornar para o menu](#container-menu)

<hr>

<a id='notas-acessos'></a>
**Controle de acessos:**

Caso também seja necessário ajustar as permissões (permitindo leitura, escrita e execução para o proprietário), você pode usar:
```bash
chmod -R 700 /home/user
```

Atenção: [Não recomendado] O comando ~~chmod -R 777~~ dá acesso total a todos usuários:
~~chmod -R 777 /home/user~~

[retornar para o menu](#container-menu)

<hr>

<a id='notas-Comandos-uteis'></a>
**Comandos uteis:**

Verificar o usuário corrente, você pode utilizar o comando:
> `whoami`

Criar um arquivo com determinado conteudo:
> `echo hi docker > docker.txt`

Verificar o conteúdo do arquivo:
> `cat docker.txt`

[retornar para o menu](#container-menu)

<br>
<br>






