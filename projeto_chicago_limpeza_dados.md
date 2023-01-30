---
title: "R Notebook"
output: html_notebook
editor_options: 
  chunk_output_type: inline
---

# INTRODUÇÃO

Esse arquivo lista as etapas que segui para minerar e limpar os conjuntos de dados usados no projeto.

# PACOTES

### Instalando e carregando pacotes:

```{r}
#install.packages("tidyverse")
library(tidyverse)
```

# WORKING DIRECTORY

### Movendo o working directory para pasta do projeto:

```{r}
setwd("C:/R Studio/Projeto_Chicago_2008_2012")
```

# CRIME DATA DE CHICAGO

### Origem do arquivo:

[Link](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-Present/ijzp-q8t2?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01)

### Carregando original para um dataframe:

```{r}
# arquivo grande, cerca de 1.7GB
crime_data_original <- read_csv("https://data.cityofchicago.org/api/views/ijzp-q8t2/rows.csv?accessType=DOWNLOAD")
```

### Filtrando dados:

```{r}
# removendo anos irrelevantes do dataframe
crime_data_filtered <- filter(crime_data_original, Year > 2007 & Year < 2013)

# escolhendo quais colunas vão servir para o projeto
keep <- c("ID", "Date", "Primary Type", "Description", "Location Description", "Arrest", "Domestic", "Community Area", "Year")

# filtrando colunas irrelevantes e mantendo apenas as colunas escolhidas
# criei um novo dataframe por motivos de segurança e organização
crime_data_filtered2 <- crime_data_filtered[keep]
```

### Usando summary() para investigar o dataframe:

```{r}
# Primeiramente, vamos dar uma olhadinha se tem algo muito estranho no dataframe. 
# Podemos observar que algumas linhas estão vazias na coluna "community area", e isso pode atrapalhar na análise.
# Existem pacotes melhores para investigar e estudar dataframes, para o propósito deste projeto, summary() é suficiente
summary(crime_data_filtered2)
```

### Removendo valores vazios do dataframe:

```{r}
# como o arquivo original é muito grande, em vez de procurar os valores corretos para as linhas NA da community area, eu resolvei apenas exclui-las
# 854 linhas foram removidas
crime_data_filtered3 <- filter(crime_data_filtered2,!is.na(crime_data_filtered2[8]))
```

### Procurando por valores duplicados em ID:

```{r}
# contando número de linhas totais
nrow(crime_data_filtered3)

# contando cada valor unique na coluna ID, cada valor na coluna Id deve ser único
# se retorna o mesmo número de linhas do dataframe, indica que tudo está correto
length(unique(crime_data_filtered3$ID))

# verificando se algum valor é duplicado. Se retornar TRUE, não tem valores duplicados
length(unique(crime_data_filtered3$ID)) == nrow(crime_data_filtered3)
```

### Carregando dados limpos para o csv:

```{r}
write_csv(crime_data_filtered2,"crime_data_chicago_2008_2012.csv")
```

# CENSUS DATA DE CHICAGO

### Origem do arquivo:

[Link](https://data.cityofchicago.org/Health-Human-Services/Census-Data-Selected-socioeconomic-indicators-in-C/kn9c-c2s2?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2022-01-01)

### Carregando dados do census para um dataframe:

```{r}
census_original <- read_csv("https://data.cityofchicago.org/resource/kn9c-c2s2.csv")
```

### Limpando os dados:

```{r}
# excluindo ultima linha porque representa o estado inteiro
census_filtered <- filter(census_original, !is.na(ca))

# mudando nome das colunas
names(census_filtered)[names(census_filtered) == 'ca'] <- 'community_area_number'
```

### Carregando dados em csv:

```{r}
write_csv(census_filtered,"census_chicago_2008_2012.csv")
```

# Conclusão

O arquivo original é muito grande (cerca de 1.7GB no momento deste projeto) e é constantemente atualizado com novas informações. Por conta disso, optei por remover valores irrelevantes para o projeto e removi linhas com campos vazios também. Salvei os dataframes resultantes em csv para uso futuro, mas em todo caso, é possivel obte-los novamente seguindo os passos descritos neste documento.

#### Bruno Cardoso de Mello

#### brunomelloac\@hotmail.com

#### 30/01/2023

Versão: 1.0
