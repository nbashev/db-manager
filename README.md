# DB Manager
PDO wrapper to simplify db queries.
It provides a simple way to create, retrieve, update & delete records.

[![Latest Stable Version](https://poser.pugx.org/h2lsoft/db-manager/v/stable)](https://packagist.org/packages/h2lsoft/db-manager)
[![License](https://poser.pugx.org/db-manager/db-manager/license)](https://packagist.org/packages/h2lsoft/db-manager)

## Requirements

- php >= 7.0
- php PDO extension


## Installation

Install directly via [Composer](https://getcomposer.org):
```bash
$ composer require h2lsoft/db-manager
```

## Basic Usage

```php
use \h2lsoft\DBManager;

$DBM = new DBManager\DBManager();
$DBM->connect('mysql', 'localhost', 'root', '', 'mydatabase');

// execute simple query with binding
$sql = "SELECT Name, SurfaceArea FROM Country WHERE Continent = :Continent AND deleted = 'NO' ORDER BY SurfaceArea DESC LIMIT 3";
$results = $DBM->query($sql, [':Continent' =>  'Asia'])->fetchAll();

// or use short version
$sql = $DBM->select("Name, SurfaceArea")
            ->from('Country')
		   ->where("Continent = :Continent")
		   ->orderBy('SurfaceArea DESC')
		   ->limit(3)
		   ->getSQL();

$DBM->close();
```

## CRUD operations

### INSERT

```php
$values = [];
$values['Name'] = "Agatha Christies";
$values['Birthdate'] = "1890-10-15";

$ID = $DBM->table('Author')->insert($values);
```


### UPDATE

```php
$values = [];
$values['Name'] = "Agatha Christies";
$affected_rows = $DBM->table('Author')->update($values, $ID);
```

### DELETE

```php
$affected_rows = $DBM->table('Author')->delete(["ID = ?", $ID]);
```


## Pagination component

```php
$sql = $DBM->select("*")
		   ->from('Country')
		   ->where("Continent = :Continent")
		   ->getSQL();

$params = [':Continent' => 'Asia'];

$current_page = 1;

// return a complete array
$pager = $DBM->paginate($sql, $params, $current_page, 20);
```

```
Array
(
    [total] => 51
    [per_page] => 20
    [last_page] => 3
    [current_page] => 1
    [from] => 1
    [to] => 20
    [page_start] => 1
    [page_end] => 3
    [data] => Array(
                        ....
                    )         
)
```



## License

MIT. See full [license](LICENSE).