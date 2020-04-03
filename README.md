# doliSQL
Helper tool for pasting SQL queries in Dolibarr code

# Use case
Say you have a hard-coded SQL query that you have edited and fine-tuned in your favorite database manager tool, working directly on test Dolibarr database.

Now that your request is perfect, you want to use it in a PHP function and preserve the visual comfort of your formatting (especially your indentation). Open `dolisql.html` in your browser, paste your SQL code in the top textarea and it will spit out the same query, formatted so that it can be pasted as PHP code (with all hard-coded `llx_` replaced with `MAIN_DB_PREFIX`).

# Example
You paste this:
```
select ec.rowid, ec.fk_socpeople from llx_element_contact as ec
    inner JOIN llx_propal as propal ON propal.rowid = ec.element_id
    INNER JOIN llx_c_type_contact as type ON type.rowid = ec.fk_c_type_contact
    INNER join llx_user as user ON user.rowid = ec.fk_socpeople
WHERE type.code = 'SALESREPFOLL';
```
You get this: 
```php
$sql = /** @lang SQL */
     'SELECT ec.rowid, ec.fk_socpeople FROM '.MAIN_DB_PREFIX.'element_contact AS ec'
     . '     INNER JOIN '.MAIN_DB_PREFIX.'propal AS propal ON propal.rowid = ec.element_id'
     . '     INNER JOIN '.MAIN_DB_PREFIX.'c_type_contact AS type ON type.rowid = ec.fk_c_type_contact'
     . '     INNER JOIN '.MAIN_DB_PREFIX.'user AS user ON user.rowid = ec.fk_socpeople'
     . ' WHERE type.code = \'SALESREPFOLL\';';
```

# Caveat / Disclaimer
This only uses regexp replacements, no actual parsing. This means you may get unexpected case changes if one of your table field name happens to be an SQL keyword (for instance RIGHT, LEFT, JOIN, etc.), even if you surround it with backticks.

It doesn’t pretty-print SQL either. It does escape single quotes.
