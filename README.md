# Curso Básico de Git e Github 

## Sumário
1. [O que é Git e controle de versão](#1-o-que-é-git-e-controle-de-versão)
2. [Git vs GitHub](#2-git-vs-github)
3. [Arquitetura: local e remoto](#3-arquitetura-local-e-remoto)
4. [Instalação e configuração inicial](#4-instalação-e-configuração-inicial)
5. [Criando repositórios (init e clone)](#5-criando-repositórios-init-e-clone)
6. [Acompanhando mudanças (status)](#6-acompanhando-mudanças-git-status)
7. [Preparando mudanças (add)](#7-preparando-mudanças-git-add)
8. [Salvando mudanças (commit)](#8-salvando-mudanças-git-commit)
9. [Removendo arquivos (rm)](#9-removendo-arquivos-git-rm)
10. [Histórico (log)](#10-histórico-git-log)
11. [Branches e merge](#11-branches-e-merge)
12. [Viajando entre commits e comparando (checkout e diff)](#12-viajando-entre-commits-e-comparando)
13. [Enviando e recebendo (push, fetch, pull)](#13-enviando-e-recebendo-push-fetch-pull)
14. [Descartando mudanças (restore)](#14-descartando-mudanças-git-restore)
15. [Guardando trabalho inacabado (stash)](#15-guardando-trabalho-inacabado-git-stash)
16. [Desfazendo commits com segurança (revert)](#16-desfazendo-commits-com-segurança-git-revert)
17. [Histórico limpo (rebase)](#17-histórico-limpo-git-rebase)
18. [Pull requests no GitHub](#18-pull-requests-no-github)
19. [Cheat sheet de comandos](#19-cheat-sheet-de-comandos)

---
## 1. O que é Git e controle de versão

Git é uma ferramenta que acompanha continuamente cada mudança feita nos seus arquivos: o que mudou, quando mudou, quem mudou. Funciona com quase qualquer tipo de arquivo (código em JavaScript, PHP, Python, textos, imagens, até vídeos), embora seja mais usado em projetos de programação.

O ponto mais poderoso é que o Git guarda versões diferentes dos seus arquivos. Você pode voltar a qualquer versão anterior em instantes, sem medo de sobrescrever algo importante. Por isso ele é chamado de **sistema de controle de versão**.

### Exemplo do dia a dia
Você entrega um projeto e o cliente aprova. Um mês depois ele pede alterações e você as faz. Dias depois ele diz: *"a versão antiga era melhor"*. Sem controle de versão, o código original já foi sobrescrito. Com Git, basta voltar ao estado anterior.

---

## 2. Git vs GitHub

| Característica | Git | GitHub |
| :--- | :--- | :--- |
| **Natureza** | Ferramenta de controle de versão | Plataforma online para hospedar repositórios Git |
| **Onde roda** | No seu computador (local) | Na nuvem (remoto) |
| **Função** | Registra o histórico de mudanças | Centraliza o trabalho da equipe, facilita compartilhar e colaborar |

Se várias pessoas trabalham no mesmo projeto, cada uma tem sua própria versão no computador. É preciso um lugar central onde todos enviem suas atualizações: o GitHub. Existem alternativas, como GitLab e Bitbucket, mas o GitHub (hoje da Microsoft) é o mais popular, especialmente em projetos de código aberto.

---

## 3. Arquitetura: local e remoto

O Git se divide em duas partes: **local** (seu computador) e **remota** (a nuvem, normalmente o GitHub).

### O fluxo local em três etapas

| Etapa | O que é |
| :--- | :--- |
| **1. Working directory** *(diretório de trabalho)* | A pasta do projeto onde você cria e edita arquivos. |
| **2. Staging area** *(área de preparação)* | Espaço intermediário onde você coloca as mudanças que estão prontas para o próximo passo. Permite revisar, ajustar ou remover antes de salvar. |
| **3. Repository** *(repositório local)* | Onde todas as versões e o histórico completo ficam guardados. Você chega aqui com o commit. |

```text
Working directory --(git add)--> Staging area --(git commit)--> Repositório local --(git push)--> GitHub (remoto)
```

Até o commit, tudo acontece só no seu computador. A nuvem entra quando você quer compartilhar, acessar de outra máquina ou ter um backup do código, como o Google Drive ou o OneDrive para arquivos comuns.

> **Por que existe a staging area?**  
> Para conferir e corrigir o que estiver errado. O commit só acontece quando você tem certeza de que está tudo certo.

---

## 4. Instalação e configuração inicial

- Pesquise "Git" e acesse o site oficial, na seção de download.
- **Windows:** escolha a versão 32 ou 64 bits e execute o instalador. Ele também instala o Git Bash, um terminal parecido com o do Linux.
- **Mac:** siga as instruções, inclusive a opção com Homebrew.
- **Linux:** siga o guia da sua distribuição.

Abra o terminal e verifique a instalação:

```bash
git --version
```

Se aparecer um número de versão, está instalado. Caso contrário, você verá uma mensagem de erro.

### Identificando-se (obrigatório no primeiro commit)

Na primeira vez que fizer um commit, o Git pode pedir que você diga quem é. Esses dados ficam registrados em cada commit do histórico.

```bash
git config --global user.email "seu@email.com"
git config --global user.name "Seu Nome"
```

A opção `--global` vale para o computador inteiro. Use `--local` para valer apenas no repositório atual.

---

## 5. Criando repositórios: init e clone

Há duas maneiras de ter um repositório Git. Em ambas, o resultado é uma pasta oculta `.git`.

### 5.1 Inicializar localmente: `git init`

```bash
cd desktop
mkdir gitCursoPet
cd gitCursoPet
touch 1.txt 2.txt
mkdir my-folder
git init
```

O Git responde `Initialized empty Git repository`. Com `ls -la` você vê a pasta oculta `.git`: é o coração do projeto, onde o Git guarda tudo (mudanças, autores, versões anteriores). Quem a cria é o próprio Git.

### 5.2 Criar no GitHub e clonar: `git clone`

1. Em [github.com](https://github.com), clique em **New**, dê um nome (ex.: `git-CursoPet`), escreva uma descrição, escolha **Public** e clique em **Create repository**.
2. Crie arquivos direto pelo site (*Create a new file*) e clique em **Commit changes** (*commit* significa "salvar").
3. Clique em **Code** e copie o link HTTPS.

```bash
git clone https://github.com/usuario/gitCursoPet.git
cd git-journey
```

O repositório vem da nuvem para o seu computador, com todos os arquivos e a pasta `.git`.

---

## 6. Acompanhando mudanças: `git status`

Mostra o que mudou na *working directory*: arquivos modificados, novos (*untracked*), deletados e o que já está na *staging area*.

```bash
git status
```

> **Termos:** arquivos *tracked* já são conhecidos pelo Git (vieram do repositório). Arquivos *untracked* são novos, ainda desconhecidos para ele.

---

## 7. Preparando mudanças: `git add`

Mover mudanças da *working directory* para a *staging area* se chama *adding*. É dizer ao Git: *"quero manter esta mudança"*.

| Comando | O que faz |
| :--- | :--- |
| `git add --all` | Prepara todas as mudanças do projeto, incluindo arquivos deletados. |
| `git add -A` | Idêntico ao `--all`. |
| `git add .` | Prepara as mudanças da pasta atual e de tudo dentro dela. Se você estiver em uma subpasta, só ela é incluída. |
| `git add *` | Prepara os arquivos visíveis, mas não os deletados. |
| `git add arquivo.txt` | Prepara só aquele arquivo. |
| `git add pasta/3.txt` | Prepara um arquivo dentro de uma pasta. |
| `git add *.txt` | Prepara todos os `.txt` da pasta atual, sem deletados nem subpastas. |

> **Boa prática:** para preparar tudo de uma vez, vá à pasta raiz do projeto e use `git add .`

### Desfazer o add: `git reset`

Tira tudo da *staging area* e devolve à *working directory* (o status mostra *Unstaged changes after reset*). Seus arquivos não são apagados.

```bash
git reset
```

---

## 8. Salvando mudanças: `git commit`

O commit salva de forma permanente o que está na *staging area* no repositório local, como uma versão registrada do histórico.

```bash
git commit -m "Descrição do que foi alterado"
```

A opção `-m` define a mensagem. Depois do commit, o terminal mostra quantos arquivos e linhas mudaram, e `git status` indica *"nothing to commit, working tree clean"*. Para novas mudanças, repita: `add` e depois `commit`.

### Desfazer o último commit

```bash
git reset HEAD~1
```

Desfaz o último commit e devolve as mudanças à *working directory*, prontas para serem editadas ou commitadas de novo.

### Tipos de reset

| Comando | Efeito |
| :--- | :--- |
| `git reset` | Tira mudanças da *staging area*. Arquivos deletados à mão não voltam. |
| `git reset --hard` | Restaura tudo: mudanças e arquivos deletados voltam ao último estado commitado. **Cuidado:** descarta alterações não salvas. |

---

## 9. Removendo arquivos: `git rm`

Apagar um arquivo manualmente e depois dar `git add` são dois passos. O `git rm` faz os dois de uma vez: deleta o arquivo e já coloca a remoção na *staging area*.

| Comando | Efeito |
| :--- | :--- |
| `git rm 4.txt` | Deleta o arquivo e prepara a remoção. Se o arquivo tem modificações não commitadas, o Git recusa. |
| `git rm -f 4.txt` | Força a remoção, mesmo com modificações locais (`--force`). |
| `git rm --cached 4.txt` | Remove só da *staging area*; o arquivo continua na pasta e vira *untracked*. |
| `git rm -r pasta` | Remove a pasta e todo o conteúdo (recursivo). Sem `-r`, não remove o conteúdo. |

---

## 10. Histórico: `git log`

```bash
git log
git log --oneline
```

Mostra todos os commits com autor, data e mensagem. Cada commit tem um ID (uma sequência longa de letras e números) usado depois para voltar a versões anteriores. Com `--oneline` o resumo é compacto, com IDs curtos que também funcionam. Para sair da tela do log, pressione `q`.

---

## 11. Branches e merge

Uma **branch** (ramo) é uma linha separada de desenvolvimento, onde você trabalha de forma independente. A branch padrão se chama `main` e é a linha central do projeto.

> **Analogia da cozinha:** a `main` é a cozinha principal de um restaurante, de onde saem os pratos servidos. Para testar uma receita nova, você usa uma cozinha de testes (outra branch). Quando a receita estiver perfeita, leva à cozinha principal. Assim, a `main` fica segura.

**Merge** significa combinar as mudanças de duas branches em uma. Você pode ter várias branches (`staging`, `development`, `front-end`, `back-end`) e juntá-las à `main` quando estiverem testadas.

| Comando | Função |
| :--- | :--- |
| `git branch` | Lista as branches. A atual tem um asterisco (`*`). |
| `git branch development` | Cria a branch `development`, que herda o estado exato da branch em que você está. |
| `git checkout development` | Muda para a branch `development`. |
| `git merge development` | Traz para a branch atual as mudanças de `development`. |

### Conflitos de merge

Um conflito acontece quando o mesmo trecho do mesmo arquivo foi alterado de formas diferentes em duas branches. O Git não sabe qual versão manter e deixa a decisão para você.

---

## 12. Viajando entre commits e comparando

### Voltar a um commit anterior

```bash
git log --oneline
git checkout <id-do-commit>
```

O projeto volta ao estado exato daquele commit, e o terminal mostra `HEAD detached at ...`. Antes, todas as suas mudanças precisam estar commitadas. Para voltar ao presente:

```bash
git checkout main
```

### Comparar commits: `git diff`

```bash
git diff <id-mais-recente> <id-mais-antigo>
```

Mostra o que foi removido (em vermelho) e adicionado (em verde). 

---

## 13. Enviando e recebendo: push, fetch, pull

| Comando | O que faz |
| :--- | :--- |
| `git push origin main` | Envia os commits locais para a branch `main` do remoto. `origin` é o repositório remoto. |
| `git fetch` | Baixa as novidades do remoto para o repositório local, mas sem alterar seus arquivos. |
| `git pull` | Faz `fetch` + `merge`: baixa e já atualiza a *working directory*. |

Cada branch precisa ser enviada separadamente. Por exemplo, `git push origin staging` cria a branch `staging` no remoto e envia seus commits.

* *push, fetch e pull são os comandos mais frequentes no trabalho diário.*

---

## 14. Descartando mudanças: `git restore`

Você começou uma funcionalidade, mexeu em 10 ou 15 arquivos e percebeu que a abordagem não funciona. Desfazer tudo à mão seria praticamente impossível. O `git restore` volta arquivos ou pastas ao estado do último commit.

| Comando | Efeito |
| :--- | :--- |
| `git restore 1.txt` | Descarta as mudanças não commitadas daquele arquivo. |
| `git restore pasta` | Restaura uma pasta inteira. |
| `git restore .` | Restaura todos os arquivos do repositório. |
| `git restore --staged arquivo` | Tira o arquivo da *staging area*, mantendo as edições na *working directory*. |
| `git restore --staged .` | Tira tudo da *staging area*. |

> **Atenção:** `git restore` sem `--staged` descarta suas edições não commitadas. Elas **não** podem ser recuperadas.

---

## 15. Guardando trabalho inacabado: `git stash`

Você está no meio de uma funcionalidade grande, ainda não commitável, e precisa trocar de branch para revisar algo. O Git bloqueia a troca (*Your local changes would be overwritten by checkout*). Em vez de jogar o trabalho fora, use o **stash**, uma área temporária.

| Comando | Efeito |
| :--- | :--- |
| `git stash` | Guarda as mudanças não commitadas. |
| `git stash list` | Lista os stashes (`stash@{0}`, `stash@{1}`, ...). O mais novo fica no topo. |
| `git stash pop` | Restaura o stash mais recente e o remove da lista. |
| `git stash apply` | Restaura o stash mas mantém na lista, para reutilizar. |
| `git stash drop` | Remove um stash da lista. |
| `git stash pop stash@{0}` | Aplica e remove um stash específico (também vale para `apply`). |

---

## 16. Desfazendo commits com segurança: `git revert`

O `git revert` desfaz as mudanças de um commit antigo, mas sem apagá-lo: cria um novo commit que inverte aquelas mudanças. O histórico permanece limpo e rastreável. 

### Reset vs Revert

| Característica | `git reset` | `git revert` |
| :--- | :--- | :--- |
| **O que faz** | Volta a um commit e descarta os posteriores | Cria um novo commit que inverte um commit específico |
| **Histórico** | Commits removidos somem do log | Fica registrado que houve reversão |
| **Em repositórios compartilhados** | Pode confundir os colegas | Mais seguro: todos veem o commit de reversão |

---

## 17. Histórico limpo: `git rebase`

Você cria a branch `feature` a partir da `main` e trabalha nela. Enquanto isso, a `main` recebe atualizações e você quer incorporá-las. Duas opções:

- **Merge:** cria um commit de merge extra. Várias vezes seguidas, o histórico fica poluído.
- **Rebase:** muda a base da sua branch. Os commits novos da `main` são aplicados primeiro, e os seus são reaplicados por cima, resultando em um histórico linear e limpo.

### O que acontece por baixo

1. O Git encontra o último commit em comum entre as duas branches.
2. Guarda temporariamente os commits da `feature` feitos depois dele.
3. Aplica os novos commits da `main`.
4. Reaplica os commits da `feature` por cima, um a um.

> **Cuidado:** o rebase reescreve o histórico e muda os IDs dos commits. Não use em branches públicas ou compartilhadas sem avisar a equipe, pois as cópias locais de outras pessoas deixam de bater. Em branches pessoais, só suas, é seguro. Evite reescrever commits que já estão em uma branch remota compartilhada.

---

## 18. Pull requests no GitHub

Um **pull request (PR)** é um pedido para mesclar suas mudanças em outra branch, geralmente a `main`: *"fiz alterações na minha branch; revisem e, se estiver tudo bom, façam o merge"*. Você não altera diretamente o repositório de outras pessoas, então pede permissão assim.

### Passo a passo

1. No repositório, abra a aba **Pull requests** e clique em **New pull request**.
2. Escolha **base** (branch de destino, ex.: `main`) e **compare** (branch de origem, ex.: `development`).
3. O GitHub mostra os arquivos e linhas alteradas. Clique em **Create pull request**.
4. Dê um título (ex.: *Merge development updates into main*) e uma descrição.
5. Abra o PR. Há três abas: **Conversation** (discussão), **Commits** e **Files changed**.
6. Após a revisão, clique em **Merge pull request** e depois em **Confirm merge**.

Assim, toda mudança é revisada antes de entrar na `main`, que se mantém estável, e a equipe colabora com segurança. É assim que empresas de software usam Git e GitHub.

---

## 19. Cheat sheet de comandos

| Comando | Para que serve |
| :--- | :--- |
| `git --version` | Verifica a instalação |
| `git config --global user.name / user.email` | Define sua identidade |
| `git init` | Cria um repositório local |
| `git clone <url>` | Copia um repositório remoto |
| `git status` | Mostra o estado atual |
| `git add .` / `-A` / `arquivo` | Envia mudanças à staging area |
| `git commit -m "msg"` | Salva as mudanças no repositório local |
| `git reset` / `reset --hard` | Tira da staging / restaura tudo |
| `git reset HEAD~1` | Desfaz o último commit |
| `git rm [-f \| --cached \| -r]` | Remove arquivos |
| `git log [--oneline]` | Mostra o histórico |
| `git branch [nome]` | Lista ou cria branches |
| `git checkout <branch \| id>` | Troca de branch ou de commit |
| `git merge <branch>` | Combina branches |
| `git diff <id1> <id2>` | Compara commits |
| `git push origin <branch>` | Envia ao remoto |
| `git fetch` | Baixa novidades sem mesclar |
| `git pull` | Fetch + merge |
| `git restore [--staged]` | Descarta edições / tira da staging |
| `git stash [pop \| apply \| list \| drop]` | Guarda trabalho inacabado |
| `git revert <id>` | Desfaz um commit criando outro |
| `git rebase <branch>` | Reaplica commits sobre outra base |
