# Exercícios

## Exercício 1
Encontre o preço médio arredondado com 2 casas decimais dos produtos em cada uma das categorias.
```sql
    Resposta:
        select p.category_id, ROUND(avg(price), 2) as total_price from products p
        inner join categories c on p.category_id = c.category_id
        group by p.category_id    Resposta:
        select p.category_id, ROUND(avg(price), 2) as total_price from products p
        inner join categories c on p.category_id = c.category_id
        group by p.category_id
```
## Exercício 2
Busque todas as informações sobre os produtos que nunca foram comprados (inclusive a descrição da categoria e todos os dados do fornecedor).
```sql

   Resposta:
        with teste as (select * from products p 
        where p.product_id not in (select oi.product_id from order_items oi)), merge_product_category as
        (select ad.product_id, ad.product_name, ad.description, ad.price, ad.category_id, ad.supplier_id, c.category_name  from teste ad inner join categories c on ad.category_id = c.category_id)
        select sa.product_id, sa.product_name, sa.description, sa.price, sa.category_id, sa.supplier_id, sa.category_name, s.supplier_name , s.supplier_email, s.supplier_phone, s.supplier_address 
        from merge_product_category sa left join suppliers s on sa.supplier_id = s.supplier_id
```
## Exercício 3
Encontre os top 5 clientes que mais gastaram dinheiro em compras, exibindo o nome completo e o valor gasto formatado como dinheiro. 

Dica: Formatar como dinheiro pode ser feito assim usando o comando `CAST(xxxxxx AS MONEY)`, como em `SELECT CAST(1000 AS MONEY)`
```sql
    Respostas
      with value_filter_products as (select concat(c2.first_name, ' ', c2.last_name) as name_complete, sum((oi.quantity * oi.price)) as total_price 
      from order_items oi 
      inner join products pr on pr.product_id = oi.product_id 
      inner join customers c2 on oi.order_id = c2.customer_id
      group by pr.product_name, c2.first_name, c2.last_name), value_filter_price as (select name_complete, sum(total_price) as price_total_cliente from value_filter_products group by 
      name_complete)
      select name_complete, cast(price_total_cliente as money) as price_cliente from value_filter_price group by name_complete, price_total_cliente order by name_complete asc

```