# Docker

+ O Repositório do Docker é chamado de **Docker Hub**.

## Principais Comandos

| Comando | Descrição |
| :--- | :--- |
| `docker pull hello-world` | Baixar a imagem "hello-world" do Hub |
| `docker run hello-world`  | Executar uma imagem |
| `docker run ubuntu sleep 1d` | Executar um comando em uma imagem ¹ |
| `docker exec -it ubuntu bash` | Executar comandos no container em execução - nesse caso abre o bash no ubuntu |
| `docker start ubuntu`   | Iniciar imagem |
| `docker start b4b5af96bfb1` | Iniciar imagem (container id) |
| `docker stop ubuntu` | Parar imagem |
| `docker stop b4b5af96bfb1` | Parar imagem (container id) |
| `docker stop -t=0 ubuntu` | Parar imagem (parar imediatamente) ² |
| `docker pause ubuntu` | Pausar imagem |
| `docker pause b4b5af96bfb1` | Pausar imagem (container id) |
| `docker unpause ubuntu` | Despausar imagem |
| `docker unpause b4b5af96bfb1` | Despausar imagem (container id) |
| `docker ps` | Lista os containers em execução |
| `docker container ls` | Lista os containers em execução |
| `docker container ls -a` | Lista os containers instalados |
| `docker rm ubuntu` | Remover imagem |
| `docker rm b4b5af96bfb1` | Remover imagem (container id) |
|  |  |

¹ *o comando serve para aguardar 1 dia para desligar o ubuntu - comando do ubuntu*
² *pois normalmente ele espera 10 seg*

>1. Procura a imagem localmente
>2. Baixa a imagem caso não encontre localmente
>3. Valida o hash da imagem
>4. Executa o container.

*O `docker run` cria um novo container e o executa.*
*O `docker exec` permite executar um comando em um container que já está em execução.*

*O `docker start` é um comando utilizado para **startar** containers que estão parados, é como se ele 'acordasse' um container que está 'dormindo'.*
*Já o `docker run` é a execução de um container.*

## Docker Hub

### Nomenclatura de imagens no Hub

+ ubuntu
  + imagem oficial do hub
+ usuario/projeto
  + imagem de usuario
