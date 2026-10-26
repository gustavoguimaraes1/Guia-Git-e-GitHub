# Guia — Configurando SSH e enviando um projeto para o GitHub

Este guia mostra como configurar uma chave SSH no Linux, conectar o computador ao GitHub e enviar um projeto para um repositório remoto.

Neste exemplo, será utilizado o repositório:

```bash
git@github.com:gustavoguimaraes1/Tetris-POO.git
```

---

## 1. Verificar se já existe uma chave SSH

No terminal, execute:

```bash
ls -la ~/.ssh
```

Procure arquivos como:

```text
id_ed25519
id_ed25519.pub
```

Se eles já existirem, provavelmente uma chave SSH já foi criada.

---

## 2. Criar uma chave SSH

Caso ainda não exista uma chave, execute:

```bash
ssh-keygen -t ed25519 -C "seu_email_do_github"
```

Quando aparecer:

```text
Enter file in which to save the key:
```

pressione `Enter`.

O caminho padrão será:

```text
~/.ssh/id_ed25519
```

Depois será solicitada uma senha para proteger a chave.

Você pode definir uma senha ou pressionar `Enter` para continuar sem senha.

---

## 3. Iniciar o SSH Agent

Execute:

```bash
eval "$(ssh-agent -s)"
```

Depois adicione sua chave:

```bash
ssh-add ~/.ssh/id_ed25519
```

---

## 4. Copiar a chave pública

Execute:

```bash
cat ~/.ssh/id_ed25519.pub
```

Será exibido algo parecido com:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... email@email.com
```

Copie toda essa linha.

---

## 5. Adicionar a chave ao GitHub

No GitHub, acesse:

```text
Settings
→ SSH and GPG keys
→ New SSH key
```

Em **Title**, coloque um nome para identificar o computador, por exemplo:

```text
Lenovo Arch Linux
```

Em **Key type**, escolha:

```text
Authentication Key
```

Em **Key**, cole a chave pública copiada anteriormente.

Salve.

---

## 6. Testar a conexão SSH

No terminal:

```bash
ssh -T git@github.com
```

Na primeira conexão pode aparecer:

```text
Are you sure you want to continue connecting?
```

Digite:

```text
yes
```

Se tudo estiver funcionando, aparecerá uma mensagem semelhante a:

```text
Hi gustavoguimaraes1! You've successfully authenticated,
but GitHub does not provide shell access.
```

Isso significa que a conexão SSH está funcionando.

---

# Enviando o projeto para o GitHub

## 7. Entrar na pasta do projeto

Exemplo:

```bash
cd ~/Tetris-POO
```

Verifique os arquivos:

```bash
ls
```

---

## 8. Inicializar o Git

Dentro da pasta:

```bash
git init
```

---

## 9. Adicionar o repositório remoto

Execute:

```bash
git remote add origin git@github.com:gustavoguimaraes1/Tetris-POO.git
```

Confira:

```bash
git remote -v
```

O resultado deverá ser parecido com:

```text
origin  git@github.com:gustavoguimaraes1/Tetris-POO.git (fetch)
origin  git@github.com:gustavoguimaraes1/Tetris-POO.git (push)
```

---

## 10. Caso o `origin` já exista

Se aparecer:

```text
error: remote origin already exists
```

não adicione outro `origin`.

Execute:

```bash
git remote set-url origin git@github.com:gustavoguimaraes1/Tetris-POO.git
```

---

## 11. Adicionar os arquivos

Execute:

```bash
git add .
```

Confira:

```bash
git status
```

---

## 12. Criar o commit

Execute:

```bash
git commit -m "Adiciona projeto Tetris POO"
```

---

## 13. Definir a branch principal

Execute:

```bash
git branch -M main
```

---

## 14. Enviar o projeto

Execute:

```bash
git push -u origin main
```

O parâmetro:

```text
-u
```

faz com que a branch local `main` fique associada à branch remota `main`.

Depois disso, os próximos pushes poderão ser feitos apenas com:

```bash
git push
```

---

# Fluxo normal depois da configuração

Depois que SSH e repositório já estiverem configurados, o fluxo será:

```bash
git add .
```

```bash
git commit -m "Descrição da alteração"
```

```bash
git push
```

Ou:

```bash
git add .
git commit -m "Atualiza projeto"
git push
```

---

# Erro: `fetch first`

Se aparecer:

```text
! [rejected] main -> main (fetch first)
```

significa que existem arquivos no GitHub que ainda não estão no seu computador.

Execute:

```bash
git pull origin main --rebase
```

Depois:

```bash
git push
```

---

# Erro: `unrelated histories`

Caso apareça:

```text
fatal: refusing to merge unrelated histories
```

execute:

```bash
git pull origin main --allow-unrelated-histories
```

Depois:

```bash
git add .
git commit -m "Integra arquivos locais e remotos"
git push
```

---

# Resumo

## Configurar SSH

```bash
ssh-keygen -t ed25519 -C "seu_email_do_github"

eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_ed25519

cat ~/.ssh/id_ed25519.pub

ssh -T git@github.com
```

## Enviar projeto

```bash
cd ~/Tetris-POO

git init

git remote add origin git@github.com:gustavoguimaraes1/Tetris-POO.git

git add .

git commit -m "Adiciona projeto Tetris POO"

git branch -M main

git push -u origin main
```

## Atualizações futuras

```bash
git add .
git commit -m "Descrição da alteração"
git push
```

---

## Estrutura mental do processo

O processo pode ser entendido assim:

```text
Computador
   ↓
Chave SSH
   ↓
GitHub reconhece seu computador
   ↓
Git local
   ↓
Commit
   ↓
Push
   ↓
Repositório no GitHub
```

A chave SSH identifica seu computador para o GitHub, enquanto o Git é responsável por controlar e enviar as versões dos arquivos.
