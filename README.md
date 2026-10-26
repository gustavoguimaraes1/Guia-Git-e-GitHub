# Guia — Configurando SSH, clonando e enviando um projeto para o GitHub

Este guia mostra como configurar uma chave SSH no Linux, conectar o computador ao GitHub, clonar um repositório e enviar alterações para o GitHub.

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

# Clonando o repositório do GitHub

## 7. Escolher onde o projeto será salvo

Primeiro, vá para a pasta onde deseja guardar o projeto.

Por exemplo, sua pasta pessoal:

```bash
cd ~
```

Ou uma pasta de projetos:

```bash
cd ~/Projetos
```

---

## 8. Clonar o repositório

Execute:

```bash
git clone git@github.com:gustavoguimaraes1/Tetris-POO.git
```

O Git criará automaticamente uma pasta chamada:

```text
Tetris-POO
```

Entre nela:

```bash
cd Tetris-POO
```

Confira os arquivos:

```bash
ls
```

Confira também o repositório remoto:

```bash
git remote -v
```

O resultado deverá ser semelhante a:

```text
origin  git@github.com:gustavoguimaraes1/Tetris-POO.git (fetch)
origin  git@github.com:gustavoguimaraes1/Tetris-POO.git (push)
```

Quando você utiliza `git clone`, não precisa executar novamente:

```bash
git init
```

nem:

```bash
git remote add origin ...
```

O `git clone` já cria o repositório Git local e configura o `origin`.

---

# Caso o projeto já exista no computador

Se você já criou o projeto localmente e ainda não utilizou `git clone`, siga este caminho.

## 9. Entrar na pasta do projeto

Exemplo:

```bash
cd ~/Tetris-POO
```

Verifique os arquivos:

```bash
ls
```

---

## 10. Inicializar o Git

Dentro da pasta:

```bash
git init
```

---

## 11. Adicionar o repositório remoto

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

## 12. Caso o `origin` já exista

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

# Enviando alterações para o GitHub

## 13. Verificar o estado do repositório

Execute:

```bash
git status
```

Isso mostra quais arquivos foram criados, modificados ou removidos.

---

## 14. Adicionar os arquivos

Execute:

```bash
git add .
```

Confira novamente:

```bash
git status
```

---

## 15. Criar o commit

Execute:

```bash
git commit -m "Adiciona projeto Tetris POO"
```

O commit registra uma versão dos arquivos no histórico local do Git.

---

## 16. Definir a branch principal

Caso seja um repositório novo:

```bash
git branch -M main
```

Se você utilizou `git clone`, normalmente a branch já estará configurada.

Para conferir:

```bash
git branch
```

---

## 17. Enviar o projeto

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

# Fluxo usando `git clone`

Se o repositório já existe no GitHub, o fluxo inicial é:

```bash
cd ~

git clone git@github.com:gustavoguimaraes1/Tetris-POO.git

cd Tetris-POO
```

Depois de modificar ou adicionar arquivos:

```bash
git status

git add .

git commit -m "Descrição da alteração"

git push
```

---

# Fluxo para um projeto criado no computador

Se o projeto já existe localmente:

```bash
cd ~/Tetris-POO

git init

git remote add origin git@github.com:gustavoguimaraes1/Tetris-POO.git

git add .

git commit -m "Adiciona projeto Tetris POO"

git branch -M main

git push -u origin main
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

Ou diretamente:

```bash
git add .
git commit -m "Atualiza projeto"
git push
```

---

# Atualizar o projeto antes de começar a trabalhar

Quando estiver trabalhando em um repositório que também pode ter alterações no GitHub, é recomendável atualizar sua cópia local antes de começar:

```bash
git pull
```

Depois faça suas alterações normalmente:

```bash
git add .
git commit -m "Descrição da alteração"
git push
```

O fluxo fica:

```text
git pull
   ↓
editar arquivos
   ↓
git add .
   ↓
git commit
   ↓
git push
```

---

# Erro: `fetch first`

Se aparecer:

```text
! [rejected] main -> main (fetch first)
```

significa que existem commits no GitHub que ainda não estão no seu computador.

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

## Clonar um repositório existente

```bash
cd ~

git clone git@github.com:gustavoguimaraes1/Tetris-POO.git

cd Tetris-POO
```

## Enviar um projeto local existente

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
git pull

git add .

git commit -m "Descrição da alteração"

git push
```

---

# Estrutura mental do processo

## Quando você clona um projeto

```text
GitHub
   ↓
git clone
   ↓
Repositório local
   ↓
Editar arquivos
   ↓
git add
   ↓
git commit
   ↓
git push
   ↓
GitHub
```

## Quando você cria o projeto no computador

```text
Projeto local
   ↓
git init
   ↓
git remote
   ↓
git add
   ↓
git commit
   ↓
git push
   ↓
GitHub
```

A chave SSH identifica seu computador para o GitHub.

O `git clone` copia um repositório remoto para o computador e já configura automaticamente o Git e o `origin`.

O Git controla as versões dos arquivos, enquanto `commit`, `pull` e `push` permitem sincronizar o histórico entre seu computador e o GitHub.
