# Pipelines no PentahoPDI do DRE VECTRA 


### Documentação receita operacional

- Fizemos a leitura dos dados no BD Oracle com o "Table Input" com os dados em posse no dentro do PentahoPDI fizemos o "Filter Rows" para filtrar somente os identificadores das contas contábeis, contudo teve o "Sort Rows" para alinhar a ativação com o "Group By" logo em seguida pra agregar.

- Adicionamos as "Add Constants" como as metricas que faltavam para ficar uma transformação coesa, logo em sequida "Add Sequence" pra automatizar os identificadores para ficar uma ID unica para cada valor, "Select Values" para selecionar as colunas que precisam de fato.

- Fizemos o "Merge Rows" para comparar os valores atualizados do pentahopdi com a consulta do elastic, "Switch Case" para entrada e saida dos novos dados, "Dummy" para coletar os erros que vem logo em seguida, se caso tiver, "Concat Fields" fazemos a concatenação das colunas separadas, "Modified JavaScript Value" colocamos o input das variáveis em um script "REST Client" fizemos a colocação dessa transformação para consumir para botar no elastsearch.

### Pipeline da receita operacional
![Receita Operacional](https://github.com/dataplataformteam/pipelines_pentaho/blob/main/imagens/1%20-%20receita%20operacional.png)


### Documentação deduções receita bruta

- Colocamos "Table Input" para ler do Oracle BD para o pentaho, fizemos o "Filter Rows" pra filtrar o que precisamos para mostrar os ids das contas contábeis que interessam, logo em seguida colocamos um "Sort Rows" para habilitar, em seguida o "Group By" para fazer as agregações por colunas.

- Adicionamos a "Add Constants" para gerar os as máscaras, logo em seguida adicionamos o "Add Sequence" para automatizar os identificadores para ficar único para cada conta contábil, e calculamos com o nosso objetivo no momento no "Calculator", selecionamos as colunas que importam no "Select Values" e fazemos a ordenação no "Sort Rows".

- Agora para fazer a comparação no "Merge Rows" fizemos a consulta no índice do Elastic gerado com uma API com o "REST client" com o formato no arquivo json para colocar no "JSON input"
selecionamos novamente as colunas "Select Values" e filtramos a conta contábil que interessa no "Filter Rows" para utilização do "Switch/case para adicionar novos registros no índice dentro do Elastic.

- "Dummy" para caso ocorra algum tipo de erro na operação ficar na transformação. Seguimos a logica nas explicações anteriores, e concatemos as colunas com o "Concat Fields", e logo em seguida fizemos os valores com o "Modifield JavaScript value" e jogando para um novo í,

### Documentação custo de produtos vendidos

- Lógica parecida com as demais pipeline já descritas, vou seguir com as principais transformações. 

- Colocamos o "Merge Rows" para chegar a comparação dos lançamentos que já estão no índice no elastic e dos que estão tranformados no pentahopdi, concatenamos as algumas colunas que vão ser essenciais na análise com "Concat Fields", "Modifield JavaScript value" pra colocar as variáveis do corretas do JS, por fim uma "REST client" uma API pra lançar já no indice.

### Pipeline do custo de produtos vendidos
![Custo de Produtos Vendidos](https://github.com/dataplataformteam/pipelines_pentaho/blob/main/imagens/4%20-%20custo_de_produtos_vendido.png)

### Documentação lucro bruto

- Seguindo a lógica dos demais, vou listar o que é mais importante abaixo nas transformações:

- Fizemos a leitura do banco de dados do Oracle, contudo consultamos o Elasticsearch, e com isso fizemos uma comparação com o "Merge Rows para ver os lançamentos antigos com os novos, em seguida pra completar colocamos o "Switch/ case" para fazer o transporte dos dados, e concatenamos novamente algumas colunas com o "Concat Field" e "Modifield JavaScript value" pra colocar as variáveis corretas do JS, por fim uma "REST client" uma API pra lançar já no índice.

### Pipeline do lucro bruto
![Lucro Bruto](https://github.com/dataplataformteam/pipelines_pentaho/blob/main/imagens/5%20-%20lucro_bruto.png)

### Documentação despesas operacionais 

- Aqui está o nosso transformers em formato de pipeline, vamos seguir com a mesma lógica como foi descrito nos demais, porém com alguns detalhes por ser uma coisa extensa:

- Tivemos ao total de mais de 60 transformações nesse pipeline, com uma base de 2 "Multiway merge join" com a unificação de algumas estruturas já em transformação para conseguir o resultado final desejado, com a mesma base dos demais de uma colocação do "Switch/ case" para fazer o transporte dos dados dos lançamentos financeiros, e concatenamos novamente algumas colunas com o "Concat Field" e "Modifield JavaScript value" pra colocar as variáveis do corretas do JS, por fim uma "REST client" uma API pra lançar já no índice.

- Deixo abaixo em zoom ok em 3 partes para uma visualização melhor da pipeline, aos invés dela toda, decidi dividir em 3 partes:

### Pipeline despesas operacionais part1
![Despesas operacionais](https://github.com/dataplataformteam/pipelines_pentaho/blob/main/imagens/Despesas_Operacionais_1.png)

### Pipeline despesas operacionais part2
![Despesas operacionais](https://github.com/dataplataformteam/pipelines_pentaho/blob/main/imagens/Despesas_Operacionais_2.png)

### Pipeline despesas operacionais part3
![Despesas operacionais](https://github.com/dataplataformteam/pipelines_pentaho/blob/main/imagens/Despesas_Operacionais_3.png)


### Documentação irpj csll

- Segue a mesma lógica dos demais, vou falar das transformações mais importantes:

- Colocamos o "Merge  rows" para comparar os dados transformados no pentaho com os dados do índice no elastic quando fizemos a consulta, botamos o "Switch/case" para entrada e saída dos dados antigos e entrada dos dados novos, concatenamos as colunas , e "Modifield JavaScript value" pra colocar as variáveis do corretas do JS, por fim uma "REST client" uma API pra lançar já no índice.

### Pipeline irpj csll

![Irpj csll](https://github.com/dataplataformteam/pipelines_pentaho/blob/main/imagens/8%20-%20irpj_csll.png)

### Documentação lucro prejuízo liq p

- O padrão dos anteriores na parte inicial das transformações.

-  Colocamos 4 "Multiway merge join" para fazer a junções dos valores nas estrutura das transformações, mergiamos no "Merge rows" para fazer a comparação novamente, e logo em seguida um "Switch/case" para fazer a troca dos antigos registros para os novos lançamentos financeiros, e concatenamos novamente algumas colunas com o "Concat Field" e "Modifield JavaScript value" pra colocar as variáveis do corretas do JS, por fim uma "REST client" uma API pra lançar já no índice.

### Pipeline lucro prejuízo liq p

![Lucro prejuizo liq p](https://github.com/dataplataformteam/pipelines_pentaho/blob/main/imagens/9%20-%20lucro_prejuizo_liq_p.png)


