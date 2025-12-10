# 🚀 Projeto OnPlay Reborn - Infraestrutura Vagrant & Docker

Este projeto automatiza a criação de um ambiente de desenvolvimento completo para a aplicação **OnPlay**, utilizando **Vagrant** para virtualização e **Docker** para orquestração dos contêineres (Frontend, Backend e Banco de Dados).

## 🏗️ Arquitetura do Projeto

A infraestrutura é baseada em uma máquina virtual Debian gerenciada pelo Vagrant. Dentro desta VM, o Docker Compose orquestra três serviços principais:

1.  **Frontend (Angular):**
    * Servidor web: Nginx (Alpine).
    * Porta no Container: 80.
    * Configuração: SPA (Single Page Application) com redirecionamentos corrigidos.
2.  **Backend (Java/Spring Boot):**
    * JDK: Eclipse Temurin 17 (Jammy).
    * Porta no Container: 8080.
    * Build: Maven (Multi-stage build).
3.  **Banco de Dados (PostgreSQL):**
    * Versão: 15.
    * Porta no Container: 5432.
    * Persistência: Volume Docker `postgres_data`.

### Fluxo de Rede
`Usuário (Windows/Host)` ➡️ `Vagrant (Port Forwarding)` ➡️ `Docker Container`

---

## 🔌 Portas Utilizadas

Para acessar os serviços do seu computador local (Windows), utilize as seguintes portas:

|
| **Frontend** | `4200` | `80` | [http://localhost:4200](http://localhost:4200) |
| **Backend API** | `8080` | `8080` | [http://localhost:8080/api](http://localhost:8080/api) |
| **PostgreSQL** | `5432` | `5432` | `localhost:5432` |

---

## 🚀 Como Executar o Deploy

### Pré-requisitos
* VirtualBox instalado.
* Vagrant instalado.
* Git Bash ou PowerShell.

### Passo a Passo

1.  **Clone ou baixe** este repositório.
2.  Abra o terminal na pasta raiz do projeto (onde está o `Vagrantfile`).
3.  Inicie o ambiente com o comando:
    ```bash
    vagrant up
    ```
    *O processo pode levar alguns minutos na primeira vez, pois fará o download da ISO do Debian, instalará o Docker e compilará as imagens do Java e Node.*

4.  **Acesse a aplicação:**
    Abra seu navegador e vá para: **[http://localhost:4200](http://localhost:4200)**.

### Comandos Úteis

* **Parar a máquina:**
    ```bash
    vagrant halt
    ```
* **Reiniciar e aplicar alterações (Deploy novo):**
    ```bash
    vagrant reload --provision
    ```
* **Acessar o terminal da VM:**
    ```bash
    vagrant ssh
    ```
* **Destruir tudo (Reset total):**
    ```bash
    vagrant destroy -f
    ```
* **Passo a passo do acesso:**
O sistema possui controle de acesso baseado em funções (Role Based Access Control). Abaixo estão os detalhes de como utilizar cada tipo de conta.

1. Criando uma Conta (Usuário Padrão)
Qualquer pessoa pode se cadastrar no sistema através da interface pública.

Na tela de Login, clique em "Registrar" ou "Criar Conta".

Preencha os dados solicitados (Nome, Email, Senha).

Ao finalizar, o usuário será criado com a permissão padrão de USER (Usuário Comum).

Acesso: Pode navegar pelo catálogo, assistir aos filmes e visualizar detalhes.

Restrição: Não tem permissão para cadastrar, editar ou excluir filmes.

2. Conta Administrador (Acesso Total)
Para gerenciar o catálogo, o sistema cria automaticamente uma conta de Administrador (Root/Super Admin) na primeira vez que o Backend é iniciado (Database Seeding).

Esta conta possui a role ADMIN, que libera funcionalidades exclusivas no Frontend.

Credenciais Padrão do Admin:

Login/Email: admin@admin.com (Verifique no seu DataInitializer.java se alterou) Senha: 123456 (Ou a senha definida no seed do backend)

Funcionalidades Exclusivas do Admin:

Adicionar Filmes: Acesso ao botão/formulário de cadastro de novos títulos, onde é possível inserir URL da capa, sinopse, título e URL do vídeo.

Gestão: Permissão para editar ou remover filmes existentes do catálogo.

---

## 🛠️ Solução de Problemas Comuns

* **Porta em uso (Address already in use):**
    Se houver conflito na porta 5432 (Postgres) ou 8080 (Java) no seu Windows, edite o `Vagrantfile` e altere a porta do `host`.
    *Exemplo:* `config.vm.network "forwarded_port", guest: 5432, host: 5433`

* **Frontend não carrega (Tela Branca/Erro de Conexão):**
    Tente acessar em uma aba anônima para evitar cache de redirecionamento do navegador ou force a recarga com `Ctrl + F5`.