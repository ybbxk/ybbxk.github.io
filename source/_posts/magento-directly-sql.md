title: magento直接使用sql语句
date: 2016-02-24 17:32:25
categories: [magento,php]
tags: [magento,sql]
---
原文[DIRECT SQL QUERIES IN MAGENTO](http://fishpig.co.uk/magento/tutorials/direct-sql-queries/)有点长，直接转过来了。  
Magento's use of data models provide a great way to access and modify data. Using aptly named methods and clever abstraction, Varien hide away the complex SQL needed to perform data operations. While this makes learning models easier, it often impacts the speed of the operation and therefore the responsiveness of your site. This is especially true when saving models that use the EAV architecture. More often that not, this cannot be avoided, however there are some situations where executing direct SQL queries would be simpler and much quicker leading to a more optimised Magento installation. An example of this is updating product prices globally in Magento. It would be easy enough to write some Magento code that looped through all products and modified the price. On a large data set, saving each individual product can take a long time and therefore make the system unusable. To combat this, it is possible to issue a direct SQL query which could update 1000's of products in 1 or 2 seconds.

<!-- more -->
Database Connections In Magento
By default, Magento will automatically connect to it's database and provide two separate resources which you can use to access data: core_read and core_write. As you can probably guess, core_read is for reading from the database while core_write is for writing to the database. It is important to ensure that you use the correct resource when reading or writing data to the database, especially when writing custom Magento extensions that will be released into the wild.

```php
<?php   
    /**
     * Get the resource model
     */
    $resource = Mage::getSingleton('core/resource');
    
    /**
     * Retrieve the read connection
     */
    $readConnection = $resource->getConnection('core_read');
    
    /**
     * Retrieve the write connection
     */
    $writeConnection = $resource->getConnection('core_write');
```
Table names and table prefixes
When installing Magento, you are given the option to use a table prefix. A table prefix is a string of characters that is added to the start of every table name in your database. These are useful if you are installing multiple system into 1 database as it helps to distinguish each application's data from another. Fortunately, Magento has a simple built in function which allows you to add the prefix to a given table name.

Get a table name from a string
```php
<?php

    /**
     * Get the resource model
     */
    $resource = Mage::getSingleton('core/resource');
    
    /**
     * Get the table name
     */
    $tableName = $resource->getTableName('catalog_product_entity');
    
    /**
     * if prefix was 'mage_' then the below statement
     * would print out mage_catalog_product_entity
     */
    echo $tableName;
```
Get a table name from an entity name
```php
<?php

    /**
     * Get the resource model
     */
    $resource = Mage::getSingleton('core/resource');
    
    /**
     * Get the table name
     */
    $tableName = $resource->getTableName('catalog/product');
    
    /**
     * if prefix was 'mage_' then the below statement
     * would print out mage_catalog_product_entity
     */
    echo $tableName;
```
Reading From The Database
While Magento models hide the complexity of the EAV system, they sometimes request far more data than is needed. If for example you have a product ID and want it's SKU, it would be much quicker to run a single query to obtain this value than to load in a whole product model (the inverse of this operation is available via the product resource class).

Varien_Db_Select::fetchAll
This method takes a query as it's parameter, executes it and then returns all of the results as an array. In the code example below, we use Varien_Db_Select::fetchAll to return all of the records in the catalog_product_entity table.
```php
<?php
    
    /**
     * Get the resource model
     */
    $resource = Mage::getSingleton('core/resource');
    
    /**
     * Retrieve the read connection
     */
    $readConnection = $resource->getConnection('core_read');
    
    $query = 'SELECT * FROM ' . $resource->getTableName('catalog/product');
    
    /**
     * Execute the query and store the results in $results
     */
    $results = $readConnection->fetchAll($query);
    
    /**
     * Print out the results
     */
     var_dump($results);
```
Varien_Db_Select::fetchCol
This method is similar to fetchAll except that instead of returning all of the results, it returns the first column from each result row. In the code example below, we use Varien_Db_Select::fetchCol to retrieve all of the SKU's in our database in an array.
```php
<?php
    /**
      * Get the resource model
      */
    $resource = Mage::getSingleton('core/resource');
    
    /**
     * Retrieve the read connection
     */
    $readConnection = $resource->getConnection('core_read');
    /**
     * Retrieve our table name
     */
    $table = $resource->getTableName('catalog/product');

    /**
     * Execute the query and store the results in $results
     */
    $sku = $readConnection->fetchCol('SELECT sku FROM ' . $table . '*');
    
    /**
     * Print out the results
     */
     var_dump($results);
```
Try this code and look at the results. Notice how all of the SKU's are in a single array, rather than each row having it's own array? If you don't understand this, try changing fetchCol for fetchAll and compare the differences.

Varien_Db_Select::fetchOne
Unlike the previous two methods, Varien_Db_Select::fetchOne returns one value from the first row only. This value is returned on it's own and is not wrapped in an array. In the code example below, we take a product ID of 44 and return it's SKU.
```php
<?php

    /**
     * Get the resource model
     */
    $resource = Mage::getSingleton('core/resource');
    
    /**
     * Retrieve the read connection
     */
    $readConnection = $resource->getConnection('core_read');

    /**
     * Retrieve our table name
     */
    $table = $resource->getTableName('catalog/product');
    
    /**
     * Set the product ID
     */
    $productId = 44;
    
    $query = 'SELECT sku FROM ' . $table . ' WHERE entity_id = '
             . (int)$productId . ' LIMIT 1';
    
    /**
     * Execute the query and store the result in $sku
     */
    $sku = $readConnection->fetchOne($query);
    
    /**
     * Print the SKU to the screen
     */
    echo 'SKU: ' . $sku . '<br/>';
```
When trying out this example, ensure you change the product ID to an ID that exists in your database!

You may think that fetchOne works the same as fetchCol or fetchAll would if you only added 1 column to the SELECT query and added a 'LIMIT 1', however you would be wrong. The main difference with this function is that the value returned is the actual value, where as Varien_Db_Select::fetchCol and Varien_Db_Select::fetchAll would wrap the value in an array. To understand this a little, try swapping the method's and comparing the results.

Writing To The Database
When saving a Magento model, there can be a lot of background data being saved that you weren't even aware of. For example, saving a product model can take several seconds due to the amount of related data saves and indexing that needs to take place. This is okay if you need all the data saving, but if you only want to update the SKU of a product, this can be wasteful.

The example code below will show you how when given a product ID, you can alter the SKU. This is a trivial example but should illustrate how to execute write queries against your Magento database.
```php
<?php

    /**
     * Get the resource model
     */
    $resource = Mage::getSingleton('core/resource');
    
    /**
     * Retrieve the write connection
     */
    $writeConnection = $resource->getConnection('core_write');

    /**
     * Retrieve our table name
     */
    $table = $resource->getTableName('catalog/product');
    
    /**
     * Set the product ID
     */
    $productId = 44;
    
    /**
     * Set the new SKU
     * It is assumed that you are hard coding the new SKU in
     * If the input is not dynamic, consider using the
     * Varien_Db_Select object to insert data
     */
    $newSku = 'new-sku';
    
    $query = "UPDATE {$table} SET sku = '{$sku}' WHERE entity_id = "
             . (int)$productId;
    
    /**
     * Execute the query
     */
    $writeConnection->query($query);
```
To test this has worked, use the knowledge gained from the first part of this tutorial to write a query to extract the SKU that has just been changed.

Varien_Db_Select
The Varien_Db_Select, which has been touched on in this article is a far better option for extracting/wriiting information. Not only is it easy to use, it also provides a layered of security, which if used correctly, is impenetrable. More will be covered on Varien_Db_Select (aka Zend_Db_Select) in a future article.

Conclusion
Sometimes it is necessary to execute direct SQL queries in Magento, however, please be careful! The Magento model's are there for a reason and provide a layer of security which you will have to manually add to your own direct SQL queries. Be sure to escape any user input and when possible, stick to the Magento model methods! If you can't stick to the Magento models, consider using Varien_Db_Select; it won't stop you making errors but it will add an almost impenetrable layer of security to your database queries.

As a side note, if you're going to be querying the database directly, it would be a good idea to learn about Magento's EAV database architecture.
[DIRECT SQL QUERIES IN MAGENTO](http://fishpig.co.uk/magento/tutorials/direct-sql-queries/)

