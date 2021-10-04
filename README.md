# Alura - Challenge BI - Semana 3 e 4 - Financeiro

# Desafio Proposto

Nesse terceiro e ultimo desafio da **Alura** através do Challenge BI (https://www.alura.com.br/challenges/bi) foi proposto a criação de dois (2) dashboard para a área financeira da **Alura Store** (empresa ficticia), sendo uma delas um **overview financeiro** para acompanhamento das métricas abaixo:

- Receita 
- Custos
- Despesas
- Lucro

e um outro para **análise de cenário**

Onde foi criado o report abaixo através do Power BI e o background pelo Figma:

**Overview financeiro**
![image](https://user-images.githubusercontent.com/62486279/135931730-dd62ae27-431b-448b-911f-5783f904973d.png)

**Análise de cenário**
![image](https://user-images.githubusercontent.com/62486279/135931070-ef61471b-d337-4a3a-a871-a3e1348a7da4.png)

Link: https://bit.ly/dashboarddefinanças

E como nos outros projetos, todo o acompanhemento foi feito via Trello:

![image](https://user-images.githubusercontent.com/62486279/135931836-584355c2-29fc-440a-b0bf-15e9e7ff2a55.png)

Link: https://trello.com/b/8gb2Kmzn/challenge-bi-semana-3-e-4

# Como o report foi criado?

## 1) ETL e Relacionamento entre tabelas

Nesse projeto foi disponibilizado bases em SQL:

![image](https://user-images.githubusercontent.com/62486279/135931933-c2b1a81a-c17e-45e9-96a7-49e66ad81194.png)

Onde foi necessario fazer a restauração utilizando o MySQL conforme passo a passo abaixo: 

https://www.alura.com.br/artigos/restaurar-backup-banco-de-dados-mysql

E para criar a conexão de dados junto ao Power BI, criamaos as seguintes Query 

**Tabela - Notas fiscais, pedidos e produtos**

~~~SQL
SELECT 
  a.id_nota,a.numero_nota,a.id_pedido,a.id_vendedor,a.valor_venda,a.frete,a.imposto,
  b.id_produto,b.quantidade,b.data_compra,
  (c.preco/100) AS preco_tratado,(c.custos/100) as custo_tratado,(c.custos/100)*b.quantidade AS custos_total

FROM 
  notas_fiscais A left join pedidos B on a.id_pedido = b.id_pedido
                  left join produtos C on b.id_produto = c.id_produto;
~~~

![image](https://user-images.githubusercontent.com/62486279/135932511-df50f06c-2bb1-4385-a29d-e9f753807cef.png)

**Tabela - Notas fiscais, pedidos e produtos**
~~~SQL
SELECT 
  id_produto,categoria_produto,preco/100 as preco_tratado,custos/100 as custos_tratado 
FROM 
  produtos;
~~~

![image](https://user-images.githubusercontent.com/62486279/135934723-34da45c1-55e1-40c1-9d63-b84e487fe7ca.png)

**Tabela - Vendedores**
~~~SQL
SELECT 
  id_vendedor,nome_vendedor 
FROM 
  vendedores;
~~~

![image](https://user-images.githubusercontent.com/62486279/135934885-256f94eb-cd94-44dc-902f-4edc38db3a33.png)

E realizando a tratativa necessaria na Query, pois os valores estavam armezado por 100

Alem de criar três (3) tabelas auxiliares:

![image](https://user-images.githubusercontent.com/62486279/135935061-6c0e90f1-0942-4e4e-ad5a-e8dd7f928c9f.png)

~~~
let
    Fonte = Date.StartOfYear (List.Min(F_notas_fiscais_pedidos[data_compra])),
    DataFinal = Date.EndOfYear (List.Max(F_notas_fiscais_pedidos[data_compra])),
    Personalizar1 = List.Dates (Fonte,Number.From(DataFinal-Fonte)+1, #duration (1,0,0,0)),
    #"Convertido para Tabela" = Table.FromList(Personalizar1, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Tipo Alterado" = Table.TransformColumnTypes(#"Convertido para Tabela",{{"Column1", type date}}),
    #"Colunas Renomeadas" = Table.RenameColumns(#"Tipo Alterado",{{"Column1", "Data"}}),
    #"Ano Inserido" = Table.AddColumn(#"Colunas Renomeadas", "Ano", each Date.Year([Data]), Int64.Type),
    #"Nome do Mês Inserido" = Table.AddColumn(#"Ano Inserido", "Nome do Mês", each Date.MonthName([Data]), type text),
    #"Colocar Cada Palavra Em Maiúscula" = Table.TransformColumns(#"Nome do Mês Inserido",{{"Nome do Mês", Text.Proper, type text}})
in
    #"Colocar Cada Palavra Em Maiúscula"
~~~

E relacionando as tabelas entre sim da seguinte forma:

![image](https://user-images.githubusercontent.com/62486279/135935234-8b429627-797d-40ed-ae15-1f67f7f6ea05.png)

## 2) Calculos 

Medida   | Dax | Comentário
-------- | ---------- | ----------

## Materiais de apoio 

**Livro:**

https://databinteligencia.com.br/produtos/dominando-o-power-bi/

**Videos:**

https://www.youtube.com/watch?v=6LkCEsWSySY
