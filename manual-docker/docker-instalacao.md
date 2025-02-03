# Instalando Docker

1. #### Atualizando o Sistema
    > sudo apt update && sudo apt upgrade -y

2. #### Instalando pré-requisitos
    Pré-requisitos: Instale pacotes que permitem o uso de HTTPS para downloads seguros:
    > sudo apt install apt-transport-https ca-certificates curl software-properties-common

3. #### Adicionando a chave GPG e o repositório oficial do Docker:

    > curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

4. #### Instalando o Docker:
    > sudo apt update

    > sudo apt install docker-ce

5. #### Verificando a instalação:
    > docker --version

6. #### Para usar o Docker sem sudo, adicione seu usuário ao grupo apropriado:
    > sudo usermod -aG docker ${USER}

    > su - ${USER}

# Instalando o Docker Compose

1. #### Criando o diretório para plugins:

    > mkdir -p ~/.docker/cli-plugins/

2. #### Baixando Docker Compose:

    ###### Verifique qual é a versão/release mais atual:
    - https://github.com/docker/compose/releases

        Exemplo: `v2.32.4`

        Insira a versão mais atual no comando a seguir:

    > curl -SL https://github.com/docker/compose/releases/download/v2.32.4/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
    chmod +x ~/.docker/cli-plugins/docker-compose

3. #### Verificando a instalação:

    > docker compose version

# Gerenciamento de Containers

1. #### Instale as seguintes extensões no vs code:

    ***Docker - Microsoft microsoft.com***
    > A extensão Docker facilita a criação, o gerenciamento e a implantação de aplicativos em contêineres do Visual Studio Code. Ela também fornece depuração de um clique de Node.js, Python e .NET dentro de um contêiner.

    ***Dev Containers - Microsoft microsoft.com***
    > A extensão Dev Containers permite que você use um contêiner Docker como um ambiente de desenvolvimento completo. 
    
    ***Kubernetes - Microsoft microsoft.com***
    > A extensão para desenvolvedores que criam aplicativos para rodar em clusters Kubernetes e para a equipe de DevOps que soluciona problemas de aplicativos Kubernetes.
    Funciona com qualquer Kubernetes em qualquer lugar (Azure, Minikube, AWS, GCP e mais!).
