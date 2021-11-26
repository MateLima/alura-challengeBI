# Desafio 2: AluraShop Dashboard de E-Commerce :package:

[Acessar o Dashboard](https://datastudio.google.com/reporting/da039d9c-f65e-4c72-82dc-10c87632fd01)



### Índice

[Estudo de caso](#estudodecaso)

[Requisitos](#requisitos)

[Base de Dados](#basededados)

[Solução](#solução)

[Contato](#Contato)

### Estudo de caso

Agora a Alura Shop **investiu** em publicidade para se destacar no mercado, e a gerência da empresa tem dúvidas se o retorno dessa propaganda **surtiu efeito**.

A nossa missão é apoiar a gerência em suas **tomadas de decisão**, e elucidar as dúvidas. Para isso desenvolveremos um dashboard estratégico de marketing com o objetivo de monitorar uma campanha de publicidade paga durante o mês de julho de 2021. Apresentaremos **indicadores relevantes** para a validação estratégica do negócio.



### Requisitos

- Total de compras
- Total de valor convertido em compras
- Total de valor investido na campanha
- Calcular o custo por clique
- Exibir a jornada de compra
- Calcular a taxa de conversão
- Mostrar o ticket médio por dispositivo
- Mostrar o retorno do investimento em publicidade (ROAS) por idade e gênero
- Calcular o valor convertido em compras por dia
- Configurar o relatório para que atualize todo dia útil as 9 horas da manhã
- Exibir data e hora da atualização



### Base de Dados

Foram disponibilizadas duas bases de dados em JSON para esse desafio, sendo elas:

- Tabelas Dispositivos
- Tabela Idade e Gênero

A **Tabela Dispositivos** apresenta informações de acesso por dispositivo e possui:

- Dia
- Plataforma Do Dispositivo
- Colocação
- Plataforma
- Alcance
- Impressões
- Quantia Gasta (Brl)
- Cliques Em Links
- Visualizações Por Página
- Compras No Website
- Compras No Facebook
- Adicionados Ao Carrinho
- Valor De Conversão
- Adicionado Ao Carrinho
- Checkouts Iniciados
- Valor De Conversão De Checkouts Iniciados
- Valor De Conversão De Compras
- Compras

A **Tabela Idade e Gênero** apresenta informações de pessoas que acessaram a página e realizaram ou não compras e possui:

- Idade
- Gênero
- Dia
- Field4
- Alcance
- Impressões
- Quantia Gasta (Brl)
- Cliques No Link
- Visualizações Por Página
- Compras No Website
- Compras No Facebook
- Adicionados Ao Carrinho
- Valor De Conversão
- Adicionado Ao Carrinho
- Checkouts Iniciados
- Valor De Conversão De Checkouts Iniciados
- Valor De Conversão De Compras
- Compras



### Solução



Utilizei o Google Data Studio para realizar esse desafio, em conjunto com o Google Spreadsheets e Google Script.

#### Passo 1: Tratamento dos dados

As bases de dados foram disponibilizadas no formato JSON, diferente de outras ferramentas o Data Studio nao possue um conector nativo para JSON, dessa forma usei o Spreadsheets para importaçao dos arquivos. 

O Spreadsheets também não se conecta nativamente com arquivos JSON. Procurando na internet ahchei um artigo do Paul Gambill que passou pelo menos desafio em uma empresa. Nesse artigo, Paul fala um script disponível no blog Fast Fedora, que gera uma nova fórmula no Google Spreadsheets para importar JSON a partir de um link.

Você pode ler o artigo [aqui](https://medium.com/@paulgambill/how-to-import-json-data-into-google-spreadsheets-in-less-than-5-minutes-a3fede1a014a) e mais sobre o projeto ImportJSON [aqui](https://blog.fastfedora.com/projects/import-json).

O script gera uma fórmula muito simples de ser utilizada:

**Imagem**

Onde **URL** é o link de origem do arquivo JSON, **query** é uma série de comandos caso você queira trazer os dados com alguma manipulação e **option** realiza algumas funções extras como a *noInherit* que desativa a herança de dados de hierarquias superiores e *noTruncate* para garantir que ser dados sejam exibidos por completo.

Após essa importação, conectei ambas tabelas no Data Studio

**Imagem 2**

Na Tabela Dispositivos fiz alterações nos tipos de dados:

- Colocação: TEXTO
- Dia: DATA
- Plataforma: TEXTO
- Plataforma do Dispositivo: TEXTO
- Quantia Gasta (Brl): MOEDA BRL
- Os demais campos foram alterados para NÚMERO

Na Tabela Idade e Gênero também houveram alterações nos tipos de dados:

- Dia: DATA
- Gênero: TEXTO
- Idade: TEXTO
- Quantia gasta (Brl): MOEDA BRL
- Os demais campos foram alterados para NÚMERO

#### Passo 2: Campos calculados e métricas

Para alcançar os requisitos do projeto, utilizei alguns campos calculados e agregações:

- **Total de compras**
- Agregração de soma: SUM(compras)

- **Total de valor convertido em compras**
- Agregação de soma: SUM(Valor De Conversão De Compras)

- **Total de valor investido na campanha**
- Agregação de soma: SUM(Quantia Gasta (Brl))

- **Calcular o custo por clique**
- Campo calculado: SUM(Quantia Gasta (Brl))/SUM(Cliques Em Links))

- **Exibir a jornada de compra**
- Para exibir esses dados, foram utilizadas a agregação de soma para os campos Visualizações por Página, Adicionados ao Carrinho, Checkouts Iniciados e Compras.

- **Calcular a taxa de conversão**
- Campos calculado: SUM(Compras)/SUM(Cliques Em Links)

- **Mostrar o ticket médio por dispositivo**
- Campo calculado: SUM(Valor De Conversão De Compras)/SUM(Compras)

- **Mostrar o retorno do investimento em publicidade (ROAS) por idade e gênero**
- Campo calculado: SUM(Valor De Conversão De Compras)/SUM(Quantia Gasta (Brl))

- **Calcular o valor convertido em compras por dia**
- Agregação de soma: SUM(Valor De Conversão De Compras)

- **Configurar o relatório para que atualize todo dia útil as 9 horas da manhã**
- O Data Studio não permite um customizaçao muito ampla para update dos dados, mas apresenta opções de atualização em períodos de 15 minutos, 4 horas ou 12 horas. Selecionei a opção para atualizar a cada 12 horas.

#### Passo 3: O Dashboard

Você pode acessar o dashboard interativo completo clicando **[AQUI](https://datastudio.google.com/reporting/da039d9c-f65e-4c72-82dc-10c87632fd01)**

**O painel Início** traz os cards abaixo:

- Compras
- Conversão em Compras
- Valor Investido
- Custo por Clique
- % de Conversão

Além de trazer um gráfico de funil para apresentar a jornada de compras, um gráfico de barras para exibir o ticket médio por dispositivo e, para uma métrica extra, um gráfico de linhas suavizado para exibir o alcance por dia por plataforma.

**imagem 3**

Já o **painel Clientes** traz os mesmos cards do painel anterior, mas algumas métricas para os clientes da plataforma, sendo elas um gráfico de barras empilhadas para exibir o ROAS por idade e gênero, um gráfico de rosca para exibir a quantidade de compras por gênero e um gráfico de linhas/séries temporais para exibir o valor de conversão de compras por dia.

**imagem 4**

### Contato

Sinta-se a vontade para me contatar:

[<img src="https://img.shields.io/badge/linkedin-%230077B5.svg?&style=for-the-badge&logo=linkedin&logoColor=white" />](https://www.linkedin.com/in/mateuslimas/)

