# Desafio 1: AluraLog Dashboard de Logística:articulated_lorry:

[Acessar o Dashboard](https://app.powerbi.com/view?r=eyJrIjoiNGVmMWI0M2EtNjE2YS00NjJjLWIyNTEtNjlmMjMxMTZjZWI4IiwidCI6IjBmMWYzNWQzLTYzODgtNDdlYS04OWU4LWE2MWViM2NkNWZmZCJ9)



### Índice

[Estudo de Caso](#estudodecaso)

[Requisitos](#requisitos)

[Base de Dados](#basededados)

[Solução](#solução)

[Contato](#Contato)



### Estudo de caso

A pessoa que gerencia a área de logística da Alura Log, está enfrentando algumas mudanças em sua área por conta do aumento da demanda dos serviços de logística no período da pandemia. Ela quer manter a qualidade de seu serviço, mas para isso precisa acompanhar constantemente as métricas do seu departamento para tomar as melhores decisões. Quando nos contou isso, analisamos que para auxiliar nesse desafio precisaremos fazer um dashboard para logística. Para isso, vamos visualizar algumas métricas muito importantes para a área.



### Requisitos

* Visualizar quantas entregas foram feitas no prazo

* Visualizar quantas entregas foram feitas atrasadas

* Mostrar o número de veículos disponíveis

* Calcular o S2D - Ship to door - expedição até a entrega, medido em dias

* Mostrar o índice de ocorrências por estado

* Visualizar o nível médio de estoque por ano

  

### Base de Dados

Foram disponibilizadas 4 bases de dados para esse desafio, sendo elas:

- Tabela de Estoque
- Tabela de Pedidos
- Tabela de Produtos
- Tabela de Veículos

A **Tabela de Estoque** apresenta *snapshots* do estoque em diversos momento do tempo e possui:

- ID Produto
- Data atualização
- Quantidade

A **Tabela de Pedidos** apresenta o registro de todos os pedidos ocorridos e possui:

- ID Pedido
- ID Produto
- Quantidade
- ID Veículo
- Status do pedido
- Data da compra
- Data da entrega
- Data previsão
- Latitude
- Longitude
- UF da entrega

A **Tabela de Produtos** apresenta todos os produtos registrados no sistema e possui:

- categoria_produto
- preço

A **Tabela de Veículos** apresenta os registros de disponibilidade atual dos veículos da AluraLog e possui:

- ID Veículos
- Tipo
- Status



### Solução

##### Passo 1: Tratamento de dados

Utilizei o editor do Power Query para efetuar os tratamentos de dados. Foram importadas todas as tabelas disponíveis.

- Tabela de Estoque:

  - Tratamento de datas: Substituição do ponto por vazio e uso da / no lugar dos traços.
    - = Table.ReplaceValue(#"Alterar Tipo",".","",Replacer.ReplaceText,{"Data atualização"})
    - = Table.ReplaceValue(#"Valor Substituído","-","/",Replacer.ReplaceText,{"Data atualização"})
  - Alteração do tipo de campo para data em formato americano.

- Tabela pedidos:

  - Alteração do tipo de campo das Datas.

- Tabela de Produtos:

  - Uso da função SPLIT para separar o ID do Nome da Categoria, que estavam junto por uso do traço.
    - Table.SplitColumn(#"Alterar Tipo", "categoria_produto", Splitter.SplitTextByDelimiter("-", QuoteStyle.Csv), {"categoria_produto.1", "categoria_produto.2"})
  - Substituição dos _ por espaço vazio no Nome da Categoria
    - = Table.ReplaceValue(#"Colunas Renomeadas","_"," ",Replacer.ReplaceText,{"categoria_produto"})
  - Alteração do tipo de Preço para decimal

- Tabela de Veículos:

  - Renomeação do campo para ID Veiculo
  - Remoção de letras do ID
    - Table.AddColumn(#"Tipo Alterado1", "Texto Após o Delimitador", each Text.AfterDelimiter([ID Veículo], "H"), type text)
  - Alteração do tipo do campo para Inteiro

  

##### Passo 2: Relacionamentos

Temos Duas fontes Principais de dados para o Dashboard.

A Pedidos tem como base da tabela pedidos e se relaciona com as tabelas de produtos e veículos. 

Os relacionamentos foram da seguinte forma:

**IMAGEM Pedidos** 




A fonte Estoque tem como base a tabela estoque  e se relaciona com a tabela de produtos.

**IMAGEM estoque** 



##### Passo 3: Medidas 

Foi feita as seguintes medidas para usar como métricas dentro do Dashboard.  

- Entregas Atrasadas:

  - Entregas Atrasadas = CALCULATE(COUNT('Tabela - Pedidos'[Staus Entrega]),'Tabela - Pedidos'[Staus Entrega]="Atrasado")

- Entregas Prazo:

  - Entregas Prazo = CALCULATE(COUNT('Tabela - Pedidos'[Staus Entrega]),'Tabela - Pedidos'[Staus Entrega]="ok")

- Entregas Pendente: (foi usado uma variável, para prevenir resultados em branco)

  - var _pendente=(CALCULATE(COUNT('Tabela - Pedidos'[Staus Entrega]),'Tabela - Pedidos'[Staus Entrega]="A Entregar"))

    return 

    IF(ISBLANK(_pendente),"--",_pendente)

- Receita:

  - Total = 'Tabela - Pedidos'[Quantidade] * RELATED('Tabela - Produtos'[preço])
  - Receita = SUM('Tabela - Pedidos'[Total])

- S2D

  - S2D = CALCULATE(AVERAGE('Tabela - Pedidos'[Dif Data]), 'Tabela - Pedidos'[Dif Data]>0)

- Veículos Disponíveis

  - Veiculos Disponiveis = CALCULATE(COUNT('Tabela - Veículos'[ID Veículo]),'Tabela - Veículos'[Status]="Disponível")  



##### Passo 4: O Dashboard

Você pode acessar o dashboard interativo completo clicando  **[AQUI](https://app.powerbi.com/view?r=eyJrIjoiNGVmMWI0M2EtNjE2YS00NjJjLWIyNTEtNjlmMjMxMTZjZWI4IiwidCI6IjBmMWYzNWQzLTYzODgtNDdlYS04OWU4LWE2MWViM2NkNWZmZCJ9)**

**O painel pedidos** atende a todos os requisitos do projeto em uma única página, trazendo:

- Cards de disponibilidade de veículos
- Cards sobre os status das entregas
- Card do valor da receita
- Filtros de ano e mês
- Estoque médio por ano exibido em um gráfico de barras
- S2D Médio exibido em um gráfico de linhas
- Quantidade de pedidos realizados por estado exibido em um mapa

**IMAGEM DASH**



**O painel produtos** traz alguns outros insights interessantes. Os cards e filtros permanecem os mesmos, mas há o acréscimo de:

- Top 5 categorias mais vendidas exibidas em Arvore Hierárquica.

**imagem DASH 2**



### Contato

Sinta-se a vontade para me contatar:

[<img src="https://img.shields.io/badge/linkedin-%230077B5.svg?&style=for-the-badge&logo=linkedin&logoColor=white" />](https://www.linkedin.com/in/mateuslimas/)





