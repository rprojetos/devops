# Docker Container

## Descrição dos principais Comandos para manipulação de containers com DOCKER

1. **`docker ps`**  
    - **Descrição:** Lista todos os containers que estão em execução no momento.  
    - **Uso:** Permite visualizar os containers ativos, com informações como container ID, imagem, status, portas e outros detalhes.

2. **`docker ps -a`**  
    - **Descrição:** Lista todos os containers, incluindo aqueles que estão parados ou finalizados.  
    - **Uso:** Útil para inspecionar o histórico de containers criados e para depuração, permitindo identificar containers que não estão mais em execução.

4. **`docker run -it appweblistdocker:v1.0.0 sh`**  
    - **Descrição:** Executa um container interativamente a partir da imagem `appweblistdocker:v1.0.0`, abrindo um terminal (`sh`) dentro do container.  
    - **Uso:** Permite acessar o shell do container para testes ou depuração.

5. **`docker run -p hostPort:containerPort appweblistdocker:v1.0.0`**  
    - **Descrição:** Inicia um container da imagem `appweblistdocker:v1.0.0` e mapeia a porta 3000 do container para a porta 3000 do host.  
    - **Uso:** Permite acessar a aplicação que roda no container através da porta 3000 do host
    **Exemplo:**
        ##### mapeamento de portas host=3000 e container=3000
        > `docker run -p 3000:3000 appweblistdocker:v1.0.0`

6. **`docker run -d -p hostPort:containerPort appweblistdocker:v1.0.0`**  
    - **Descrição:** Inicia o container em modo *detached* (em segundo plano) e faz o mapeamento da porta 3000 do host para a porta 3000 do container a partir da imagem `appweblistdocker:v1.0.0`  
    - **Uso:** Ideal para rodar a aplicação sem ocupar o terminal.
    **Exemplo:**
        ##### mapeamento de portas host=3000 e container=3000
        > `docker run -d -p 3000:3000 appweblistdocker:v1.0.0`

6. **`docker run -d -p hostPort:containerPort --name appweblist-1 appweblistdocker:v1.0.0`**  
    - **Descrição:** Inicia o container em modo *detached* (em segundo plano), faz o mapeamento da porta 3000 do host para a porta 3000 do container e nomeia o container para `appweblist-1` a partir da imagem `appweblistdocker:v1.0.0`
    - **Uso:** Ideal para rodar a aplicação sem ocupar o terminal.

7. **`docker logs -f <container_id|container_name>`**
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

8. **`docker exec <container_id|container_name> cmd`**
    - **Descrição:** 
    - **Uso:** 
    - **Exemplos:**  
      ```bash
      docker exec appweblist-1 ls
      ``` 
      ou
      ```bash
      docker exec appweblist-1 pwd
      ``` 

3. **`docker stop <container_id|container_name>`**  
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

14. **`docker rm <container_id|container_name>`**  
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