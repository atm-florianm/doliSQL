# doliSQL
Helper tool for pasting SQL queries in Dolibarr code

# Use case
Say you have a hard-coded SQL query that you have edited and fine-tuned in your favorite database manager tool, working directly on test Dolibarr database.

Now that your request is perfect, you want to use it in a PHP function and preserve the visual comfort of your formatting (especially your indentation). Open `dolisql.html` in your browser, paste your SQL code in the top textarea and it will spit out the same query, formatted so that it can be pasted as PHP code (with all hard-coded `llx_` replaced with `MAIN_DB_PREFIX`).

# Example
You paste this:
```
select product_price.price, product.price, propaldet.subprice, propaldet.buy_price_ht from llx_propaldet as propaldet
inner join llx_product as product ON propaldet.fk_product = product.rowid
left join llx_product_price as product_price on product_price.fk_product = product.rowid
where propaldet.rowid = 445;
```
You get this: 
```php
$sql = /** @lang SQL */
     'SELECT product_price.price, product.price, propaldet.subprice, propaldet.buy_price_ht FROM '.MAIN_DB_PREFIX.'propaldet AS propaldet'
     . ' INNER JOIN '.MAIN_DB_PREFIX.'product AS product ON propaldet.fk_product = product.rowid'
     . ' LEFT JOIN '.MAIN_DB_PREFIX.'product_price AS product_price ON product_price.fk_product = product.rowid'
     . ' WHERE propaldet.rowid = 445;';
```

# Caveat / Disclaimer
This only uses regexp replacements, no actual parsing. This means you may get unexpected case changes if one of your table field name happens to be an SQL keyword (for instance RIGHT, LEFT, JOIN, etc.), even if you surround it with backticks.

It doesn’t pretty-print SQL either. It does escape single quotes.
