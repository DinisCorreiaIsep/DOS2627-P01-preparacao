# Linux warm-up — DinisCorreiaIsep

Fiz os exercícios dentro da toolbox, com a pasta do repositório montada em /workspace.

Comando usado no Windows para iniciar a toolbox:

```powershell
docker run --rm -i --name dos-p01-warmup -p 18080:8080 -v "C:\Users\kkoze\Documents\escola\DOS-P01\DOS2627-P01-preparacao:/workspace" -v dos-toolbox-cache:/cache -e USER=DinisCorreiaIsep dos-toolbox bash
```

Usei a porta 18080 porque a 8080 já estava ocupada. Defini USER=DinisCorreiaIsep no arranque, pois a imagem não define essa variávelmas o utilizador Linux continuou a ser root

## 1. Criar a pasta e entrar nela

```bash
mkdir DinisCorreiaIsep
cd DinisCorreiaIsep
pwd
```

```text
/workspace/DinisCorreiaIsep
```

## 2. Criar notes.txt e mostrar a segunda linha

```bash
printf 'Primeira linha: preparar o ambiente.\nSegunda linha: testar a toolbox.\nTerceira linha: guardar as evidências.\n' > notes.txt
sed -n '2p' notes.txt
```

```text
Segunda linha: testar a toolbox.
```

## 3. Listar a pasta com permissões

```bash
ls -l
```

```text
total 0
-rw-r--r-- 1 root root 110 Oct 14 19:44 notes.txt
```

As colunas são: permissões, número de ligações, dono, grupo, tamanho, data e nome do ficheiro.

## 4. Definir APP_PORT só no shell atual

```bash
unset APP_PORT
APP_PORT=8050
printf 'Shell atual: APP_PORT=%s\n' "$APP_PORT"
bash -c 'printf "Novo shell: APP_PORT=<%s>\n" "${APP_PORT-}"'
```

```text
Shell atual: APP_PORT=8050
Novo shell: APP_PORT=<>
```

No novo shell, APP_PORT ficou sem valor.

## 5. Encontrar o processo que usa mais memória

```bash
ps -eo pid,comm,rss --sort=-rss | head -n 2
```

```text
    PID COMMAND           RSS
     12 ps               4388
```

Neste teste, o processo com mais memória foi ps, com PID 12 e 4388 KiB.

## 6. Consultar os repositórios públicos no GitHub

```bash
curl --fail --silent --show-error https://api.github.com/users/DinisCorreiaIsep | jq .public_repos
```

```text
0
```

## 7. Criar hello.sh

```bash
cat > hello.sh <<'SH'
#!/bin/sh
printf 'Hello, %s\n' "$USER"
SH
cat hello.sh
printf 'USER=%s\n' "$USER"
whoami
```

```text
#!/bin/sh
printf 'Hello, %s\n' "$USER"
USER=DinisCorreiaIsep
root
```

Criei o ficheiro com cat

## 8. Ver as permissões antes de tornar o script executável

```bash
ls -l hello.sh
```

```text
-rw-r--r-- 1 root root 39 Oct 14 19:44 hello.sh
```

## 9. Tornar o script executável e executá-lo

```bash
chmod +x hello.sh
ls -l hello.sh
./hello.sh
```

```text
-rwxr-xr-x 1 root root 39 Oct 14 19:44 hello.sh
Hello, DinisCorreiaIsep
```