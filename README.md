# Tutorial: Gerenciamento de Projetos em R

> **Criando projetos, manipulando arquivos e organizando estruturas no R**  
> Pacotes: `{fs}`, `{usethis}` e `{here}`

[![R-Ladies Goiânia](https://img.shields.io/badge/R--Ladies-Goi%C3%A2nia-purple)](https://www.rladiesgyn.com/)

---

## Olá

Organizar projetos de análise de dados é um desafio comum para quem está começando e até para quem já tem experiência em programação com R. Quantas vezes você já se deparou com códigos que funcionavam perfeitamente no seu computador, mas quebravam quando compartilhados com colegas? Ou passou horas ajustando caminhos de arquivos porque mudou a estrutura de pastas? A boa notícia é que existem pacotes que resolvem esses problemas de forma definitiva





## Sobre o projeto

Este repositório contém um **guia prático e completo** desenvolvido para a comunidade **R-Ladies Goiânia**, focado em três pilares essenciais para organização e gerenciamento de projetos em R:

- **`{usethis}`** - Criação de projetos reprodutíveis e configuração de infraestrutura
- **`{fs}`** - Manipulação segura e multiplataforma de arquivos e diretórios
- **`{here}`** - Garantia de caminhos consistentes e relativos ao projeto

### Objetivos

- Ensinar boas práticas de organização de projetos em R
- Demonstrar o uso de ferramentas modernas para gerenciamento de arquivos
- Promover reprodutibilidade e portabilidade de código
- Eliminar o uso de `setwd()` e caminhos absolutos

---

## Como usar este tutorial

### Pré-requisitos

- R (versão ≥ 4.0.0)
- RStudio (recomendado)
- Pacotes necessários:

```r
install.packages(c("usethis", "fs", "here", "quarto"))
```

### Estrutura do repositório

```
tutorial-fs-usethis-here/
├── index.qmd              # Documento principal do tutorial
├── logo.png               # Logo R-Ladies Goiânia
├── styles.css             # Estilos personalizados
├── footer.html            # Rodapé do documento
├── README.md              # Este arquivo
└── data/                  # Pasta de exemplo (criada durante tutorial)
    ├── raw/               # Dados brutos
    └── clean/             # Dados processados
```

### Passo a passo

1. **Clone ou baixe este repositório**
   ```bash
   git clone https://github.com/seu-usuario/tutorial-fs-usethis-here.git
   ```

2. **Abra o projeto no RStudio**
   - Clique duas vezes no arquivo `.Rproj`

3. **Renderize o documento**
   ```r
   quarto::quarto_render("index.qmd")
   ```

4. **Siga os exemplos interativamente**
   - Execute cada chunk de código sequencialmente
   - Pratique modificando os exemplos

---

## Tutorial

### 1. Criando projetos com `{usethis}`

Aprenda a criar projetos estruturados e configurar infraestrutura:

```r
library(usethis)

# Criar novo projeto
create_project("meu_projeto")

# Inicializar Git
use_git()

# Criar repositório no GitHub
use_github()
```

### 2. Manipulando arquivos com `{fs}`

Domine operações de arquivos de forma segura e consistente:

```r
library(fs)

# Criar estrutura de pastas
dir_create(here("data", c("raw", "clean")))
dir_create(here(c("scripts", "outputs", "figures")))

# Listar arquivos
dir_ls(here("data"))

# Copiar e mover
file_copy("origem.csv", "destino.csv")
file_move("arquivo.txt", "nova_pasta/arquivo.txt")
```

### 3. Caminhos relativos com `{here}`

Garanta portabilidade do seu código:

```r
library(here)

# Sempre use here() para caminhos
dados <- read.csv(here("data", "raw", "dados.csv"))

# Funciona em qualquer sistema operacional!
ggsave(here("figures", "grafico.png"))
```

---

## Principais funcionalidades do `{fs}`

| Função | Descrição | Exemplo |
|--------|-----------|---------|
| `dir_create()` | Cria diretórios | `dir_create("data/raw")` |
| `dir_ls()` | Lista conteúdo | `dir_ls("data", glob = "*.csv")` |
| `dir_tree()` | Visualiza estrutura | `dir_tree(here())` |
| `file_create()` | Cria arquivos | `file_create("script.R")` |
| `file_copy()` | Copia arquivos | `file_copy("a.txt", "b.txt")` |
| `file_move()` | Move/renomeia | `file_move("old.R", "new.R")` |
| `file_delete()` | Deleta arquivos | `file_delete("temp.txt")` |
| `file_info()` | Informações detalhadas | `file_info("data.csv")` |

---

## Por que usar estes pacotes?

### Problema comum

```r
# NÃO FAÇA ISSO
setwd("C:/Users/MeuNome/Documents/projeto")
dados <- read.csv("../data/arquivo.csv")
```

**Problemas:**
- Não funciona em outros computadores
- Quebra com mudança de estrutura de pastas
- Dificulta colaboração

### Solução

```r
# FAÇA ISSO
library(here)
library(fs)

dados <- read.csv(here("data", "arquivo.csv"))
file_copy(
  here("data", "raw", "dados.xlsx"),
  here("data", "clean", "dados_processados.csv"))
```

**Vantagens:**
- Funciona em qualquer máquina
- Multiplataforma (Windows, Mac, Linux)
- Código reprodutível
- Facilita colaboração

---

## Estrutura recomendada

```
meu_projeto/
├── meu_projeto.Rproj      # Arquivo de projeto
├── README.md              # Documentação
├── .gitignore             # Arquivos ignorados pelo Git
├── data/
│   ├── raw/               # Dados originais (nunca modificar!)
│   └── clean/             # Dados processados
├── scripts/
│   ├── 01-import.R        # Importação de dados
│   ├── 02-clean.R         # Limpeza e transformação
│   └── 03-analyze.R       # Análises
├── outputs/               # Resultados, tabelas
├── figures/               # Gráficos exportados
└── reports/               # Relatórios (Rmd, qmd)
```

---

## Template de projeto automatizado

Use esta função para criar projetos padronizados:

```r
criar_projeto_padrao <- function(nome) {
  # Criar projeto
  usethis::create_project(nome)
  
  # Estrutura de pastas
  fs::dir_create(here::here("data", c("raw", "clean")))
  fs::dir_create(here::here(c("scripts", "outputs", "figures", "reports")))
  
  # Scripts básicos
  fs::file_create(here::here("scripts", c(
    "01-import.R",
    "02-clean.R", 
    "03-analyze.R"
  )))
  
  # Documentação
  usethis::use_readme_md()
  
  # Git
  usethis::use_git()
  
  message("✅ Projeto criado com sucesso!")
}

# Usar:
criar_projeto_padrao("minha_analise")
```

---

## Links úteis

### Documentação oficial

- **{usethis}**: https://usethis.r-lib.org/
- **{fs}**: https://fs.r-lib.org/
- **{here}**: https://here.r-lib.org/

### Artigos e tutoriais

- [R for Data Science - Workflow: Projects](https://r4ds.hadley.nz/workflow-scripts.html#projects)
- [RStudio Projects Tutorial](https://support.posit.co/hc/en-us/articles/200526207-Using-RStudio-Projects)

### Pacotes complementares

- **{renv}** - Gerenciamento de dependências: https://rstudio.github.io/renv/
- **{targets}** - Pipelines de análise: https://docs.ropensci.org/targets/

---

## Contribuindo

Contribuições são bem-vindas! Se você encontrou um erro ou tem sugestões de melhoria:

1. Abra uma **issue** descrevendo o problema/sugestão
2. Faça um **fork** do projeto
3. Crie uma **branch** para sua feature (`git checkout -b feature/MinhaFeature`)
4. **Commit** suas mudanças (`git commit -m 'Adiciona nova feature'`)
5. **Push** para a branch (`git push origin feature/MinhaFeature`)
6. Abra um **Pull Request**

---

### Autoria

Jennifer Luz Lopes\
Engenheira Agrônoma \| Doutora em Melhoramento Genético de Plantas

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/jennifer-luz-lopes/) [![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/JenniferLopes) [![Site e Newsletter](https://img.shields.io/badge/Site%20e%20Newsletter-224573?style=for-the-badge&logo=quarto&logoColor=white)](https://jenniferlopes.quarto.pub/portifolio/)


---

## Licença

Este projeto está sob a licença MIT.
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## Agradecimentos

- **R-Ladies Global** - Por promover diversidade na comunidade R
- **R-Ladies Goiânia** - Pela oportunidade de compartilhar conhecimento
- **Posit/RStudio** - Pelas ferramentas incríveis que facilitam nosso trabalho

---

## Contato

- **R-Ladies Goiânia**:[S]
- **R-Ladies Goiânia**: [Meetup](https://www.meetup.com/rladies-goiania/) 


---

<div align="center">

**Feito com 💜 para a comunidade R**

