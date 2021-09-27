# Alura - Challenge BI - Semana 3 e 4 - Financeiro

# Desafio Proposto

# Como o report foi criado?

## 1) ETL e Relacionamento entre tabelas

## 2) Calculos 

Medida   | Dax | Comentário
-------- | ---------- | ----------

## Materiais de apoio 

**Livro:**

https://databinteligencia.com.br/produtos/dominando-o-power-bi/

**Sites:**

**Videos:**

https://www.youtube.com/watch?v=6LkCEsWSySY


~~~SQL
SELECT 
  a.id_nota,a.numero_nota,a.id_pedido,a.id_vendedor,a.valor_venda,a.frete,a.imposto,
  b.id_produto,b.quantidade,b.data_compra,
  (c.preco/100) AS preco_tratado,(c.custos/100) as custo_tratado,(c.custos/100)*b.quantidade AS custos_total

FROM 
  notas_fiscais A left join pedidos B on a.id_pedido = b.id_pedido
                  left join produtos C on b.id_produto = c.id_produto;
~~~
