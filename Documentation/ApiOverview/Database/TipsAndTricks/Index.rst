.. include:: /Includes.rst.txt

.. _database-tips-and-tricks:

=======================
Various Tips and Tricks
=======================

* Use :guilabel:`Find usages` of PhpStorm for examples! The source code of the Core is a great way to
  learn how specific methods of the API are used. In PhpStorm it is extremely helpful to right
  click on a single method and list all method usages with :guilabel:`Find usages`. This is especially handy
  to quickly see usage examples of complex methods like :php:`join()` from the `QueryBuilder`.

* `INSERT`, `UPDATE` and `DELETE` statements are often easier to read and write
  using the `Connection` object instead of the `QueryBuilder`.

* `SELECT DISTINCT aField` is not supported but can be substituted with a :php:`->groupBy('aField')`.

* :php:`getSQL()` and :php:`executeQuery()` can be used after each other during
   development to simplify debugging:

   .. code-block:: php
      :caption: EXT:some_extension/Classes/SomeClass.php

      $queryBuilder
         ->select('uid')
         ->from('tt_content')
         ->where(
            $queryBuilder->expr()->eq('bodytext', $queryBuilder->createNamedParameter('klaus'))
         );
      debug($queryBuilder->getSql());
      $result = $queryBuilder->executeQuery();

* In contrast to the old API based on :php:`$GLOBALS['TYPO3_DB']`, Doctrine DBAL will throw exceptions
  if something goes wrong when calling :php:`executeQuery()`. The exception type is a :php:`\Doctrine\DBAL\Exception`
  which can be caught and transferred to a better error message if the application has to expect
  query errors. Note this is not good habit and often indicates an architectural flaw of the application
  at a different layer.

* :php:`count()` query types using the `QueryBuilder` typically call :php:`->fetchOne()` to receive the count
  value. The :php:`count()` method of `Connection` object does that automatically and returns the count value
  result directly.
