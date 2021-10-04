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

Onde foi necessario fazer a restauração utilizando o MySQL

https://www.alura.com.br/artigos/restaurar-backup-banco-de-dados-mysql

E criado a seguinte query:

~~~SQL
SELECT 
  a.id_nota,a.numero_nota,a.id_pedido,a.id_vendedor,a.valor_venda,a.frete,a.imposto,
  b.id_produto,b.quantidade,b.data_compra,
  (c.preco/100) AS preco_tratado,(c.custos/100) as custo_tratado,(c.custos/100)*b.quantidade AS custos_total

FROM 
  notas_fiscais A left join pedidos B on a.id_pedido = b.id_pedido
                  left join produtos C on b.id_produto = c.id_produto;
~~~

Resutando na seguinte consulta:

![image](https://user-images.githubusercontent.com/62486279/135932511-df50f06c-2bb1-4385-a29d-e9f753807cef.png)


## 2) Calculos 

Medida   | Dax | Comentário
-------- | ---------- | ----------

## Materiais de apoio 

**Livro:**

https://databinteligencia.com.br/produtos/dominando-o-power-bi/

**Videos:**

https://www.youtube.com/watch?v=6LkCEsWSySY
