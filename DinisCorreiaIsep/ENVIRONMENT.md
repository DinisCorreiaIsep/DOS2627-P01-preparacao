# Ambiente de trabalho

Dinis Correia — `DinisCorreiaIsep`

## Sistema operativo

Uso Windows 11 Home, versão 10.0.26200, 64 bits, com cerca de 32 GB de RAM. O WSL está configurado com Ubuntu e WSL 2.

Já tinha Git, Docker Desktop, VS Code com o Extension Pack for Java e IntelliJ instalados. Instalei Bruno 4.2.1 e DBeaver Community 26.2.0

## Configuração do Git

Antes de alterar a configuração:

```powershell
git config --global -l
```

```text
user.name=Dinis Correia
user.email=1251672@isep.ipp.pt
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
filter.lfs.clean=git-lfs clean -- %f
```

Configurei o nome, o email e a branch principal:

```powershell
git config --global user.name "Dinis Correia"
git config --global user.email "1251672@isep.ipp.pt"
git config --global init.defaultBranch main
```

Depois:

```powershell
git config --global -l
```

```text
user.name=Dinis Correia
user.email=1251672@isep.ipp.pt
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
filter.lfs.clean=git-lfs clean -- %f
init.defaultbranch=main
```

## Docker

```powershell
docker version
```

```text
Client:
 Version:           29.5.3
 API version:       1.54
 Go version:        go1.26.4
 Git commit:        d1c06ef
 Built:             Wed Jun  3 18:03:06 2026
 OS/Arch:           windows/amd64
 Context:           desktop-linux

Server: Docker Desktop 4.78.0 (229452)
 Engine:
  Version:          29.5.3
  API version:      1.54 (minimum version 1.40)
  Go version:       go1.26.4
  Git commit:       285b471
  Built:            Wed Jun  3 17:59:56 2026
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v2.2.4
  GitCommit:        193637f7ee8ae5f5aa5248f49e7baa3e6164966e
 runc:
  Version:          1.3.5
  GitCommit:        v1.3.5-0-g488fc13e
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

```powershell
docker run --rm hello-world
```

```text
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pulling fs layer
4f55086f7dd0: Download complete
4f55086f7dd0: Pull complete
d5e71e642bf5: Download complete
Digest: sha256:11a3eb1597e864c51b3f56733cb2e8a90bf0c284bfc05f9444d81c558bc848aa
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## Teste do nginx

Iniciei o nginx e confirmei que o contentor estava a correr:

```powershell
docker run -d --name web -p 8090:80 nginx
docker ps
```

Testei a página na porta 8090:

```powershell
curl.exe --max-time 20 http://localhost:8090
```

```text
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed

  0      0   0      0   0      0      0      0                              0
100    896 100    896   0      0 300.7k      0                              0
100    896 100    896   0      0 292.4k      0                              0
100    896 100    896   0      0 285.2k      0                              0
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy, 
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional 
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

No fim, consultei os logs, parei e removi o contentor e listei as imagens:

```powershell
docker logs web
docker stop web
docker rm web
docker images
```

## Toolbox

Extraí o dos-toolbox.zip e construí a imagem na pasta que contém o Dockerfile:

```powershell
docker build -t dos-toolbox .
```

A construção terminou sem erros. Confirmei as versões:

```powershell
docker run --rm dos-toolbox versions
```

```text
git       2.53.0
java      openjdk version "25.0.4.1" 2026-08-18 LTS
docker    29.9.1 (server: 
not reachable: mount the socket)
compose   5.6.0
terraform v1.16.4
kubectl   v1.37.1 (cluster: null)
helm      v4.3.0+gbec5b06
kind      v0.33.0
ansible   ansible [core 2.21.5]
psql      18.6
```

Aparecem Java 25 e Terraform 1.16.4

## SSH

Criei uma chave Ed25519 e adicionei a chave pública à minha conta GitHub. A chave privada ficou na pasta ssh

No ficheiro ~/.ssh/config ficou esta entrada:

```sshconfig
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
```

chave pública publicada:

```powershell
curl.exe --fail --silent --show-error --max-time 20 https://github.com/DinisCorreiaIsep.keys
```

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIF8MLSyNEspZ7sLpJVN1IrymJeWj15qD6WxFdh9k1r34
```

Testei a ligação:

```powershell
ssh -T git@github.com
```

```text
Hi DinisCorreiaIsep! You've successfully authenticated, but GitHub does not provide shell access.
```

A ligação SSH funcionou.

## Problemas encontrados

- O Docker Desktop estava parado. Abri-o e repeti o teste.
- A porta 8080 estava ocupada. Usei 18080 para a toolbox.
- Os finais de linha CRLF causaram erros no bash. Repeti os comandos com LF.

## Máquina virtual e contentor

Uma máquina virtual, como a usada pelo WSL2, tem o seu próprio kernel. Um contentor Docker partilha o kernel do ambiente onde corre e costuma usar menos recursos. No Windows, o Docker usa o WSL2 para correr os contentores Linux.

## Dois nginx ao mesmo tempo

Voltei a criar web, que já tinha removido, e iniciei web2 noutra porta:

```powershell
docker run -d --name web -p 8090:80 nginx
docker run -d --name web2 -p 8091:80 nginx
docker ps
```

Estas são as linhas dos dois contentores no output de docker ps:

```text
CONTAINER ID   IMAGE                          COMMAND                  CREATED                  STATUS                  PORTS                                         NAMES
3bc588df137d   nginx                          "/docker-entrypoint.…"   Less than a second ago   Up Less than a second   0.0.0.0:8091->80/tcp, [::]:8091->80/tcp       web2
8292f79fb816   nginx                          "/docker-entrypoint.…"   1 second ago             Up 1 second             0.0.0.0:8090->80/tcp, [::]:8090->80/tcp       web
```

```powershell
docker stop web
docker stop web2
docker rm web
docker rm web2
```

## Script e Git

No Windows, registei a permissão de execução no Git:

```powershell
git add DinisCorreiaIsep
git update-index --chmod=+x DinisCorreiaIsep/hello.sh
```

```powershell
git ls-files --stage -- DinisCorreiaIsep/hello.sh
```

```text
100755 069dd9b0acd0bee728fe3505410eebeff75fb858 0	DinisCorreiaIsep/hello.sh
```

Fiz os commits com Dinis Correia e 1251672@isep.ipp.pt e enviei-os para main. Também executei:

```powershell
git pull --rebase
```

```text
Already up to date.
```

```powershell
git push
```

```text
Everything up-to-date
```
