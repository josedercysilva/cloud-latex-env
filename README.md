# ☁️ Cloud LaTeX Environment (IaC)

Este repositório implementa uma infraestrutura como código (IaC) para um ambiente de desenvolvimento e compilação LaTeX de alta performance. 

O objetivo é fornecer um *workspace* efêmero, determinístico e 100% *Zero-Touch*, eliminando a necessidade de instalações locais e o clássico problema "na minha máquina funciona". 

## 🏗️ Decisões de Arquitetura (Tech Stack)

A arquitetura foi desenhada para ser leve e imutável, utilizando conteinerização e provisionamento automatizado.

* **Orquestração:** `DevContainers` (Compatível nativamente com GitHub Codespaces e Docker local).
* **Motor de Compilação:** `Tectonic`. Substitui o monolito pesado do TeX Live (que pode ultrapassar 5GB) por um motor moderno escrito em Rust, que baixa os pacotes LaTeX do CTAN sob demanda. Ideal para containers efêmeros e integração contínua.
* **Editor Base:** `Neovim` puro, instalado via Alpine Linux.
* **Core Business (Plugin):** `VimTeX` (Injetado via dependência externa para habilitar compilação contínua e *Live Reload*).
* **Separação de Contextos (SoC):** As configurações estéticas e mapeamentos de teclado do Neovim **não** residem neste repositório. Elas são puxadas automaticamente do repositório externo de `dotfiles` durante a fase de provisionamento (`postCreateCommand`).

## ⚙️ Fluxo de Provisionamento (O Lifecycle)

Ao instanciar este container, a seguinte esteira de execução determinística ocorre nos bastidores:
1. Instalação da imagem base (`mcr.microsoft.com/devcontainers/base:alpine`).
2. Instalação de dependências do sistema operacional (`neovim`, `git`, `curl`).
3. Download e instalação do binário do `Tectonic`.
4. Clone do repositório externo de `dotfiles`.
5. Execução do script `install.sh` (garantia de estado via symlinks) para acoplar o Neovim ao motor `lazy.nvim`.

## 🚀 Como fazer o Deploy e Usar

A grande vantagem desta arquitetura é que ela não requer nenhuma configuração manual do usuário.

### Via Nuvem (Recomendado)
1. Clique no botão **`<> Code`** no topo deste repositório.
2. Acesse a aba **`Codespaces`** e crie uma nova máquina.
3. Aguarde o container iniciar (o *build* de infraestrutura ocorre automaticamente).
4. Abra o terminal integrado e inicie a edição:
   ```bash
   nvim main.tex
