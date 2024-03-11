# Laravel BATCH (BULK)
Insert and update batch (bulk) in laravel

[![License](https://poser.pugx.org/digital-splash/laravel-batch/license)](https://packagist.org/packages/digital-splash/laravel-batch)
[![Latest Stable Version](https://poser.pugx.org/digital-splash/laravel-batch/v/stable)](https://packagist.org/packages/digital-splash/laravel-batch)
[![Total Downloads](https://poser.pugx.org/digital-splash/laravel-batch/downloads)](https://packagist.org/packages/digital-splash/laravel-batch)
[![Daily Downloads](https://poser.pugx.org/digital-splash/laravel-batch/d/daily)](https://packagist.org/packages/digital-splash/laravel-batch)


# Install
`composer require digital-splash/laravel-batch`

# Service Provider
file app.php in array providers :

`DigitalSplash\Batch\BatchServiceProvider::class,`

# Aliases
file app.php in array aliases :

`'Batch' => DigitalSplash\Batch\BatchFacade::class,`

# Example Update 1

```php
use App\Models\User;

$userInstance = new User;
$value = [
     [
         'id' => 1,
         'status' => 'active',
         'nickname' => 'digital'
     ] ,
     [
         'id' => 5,
         'status' => 'deactive',
         'nickname' => 'splash'
     ] ,
];
$index = 'id';

Batch::update($userInstance, $value, $index);
```

# Example Update 2

```php
use App\Models\User;

$userInstance = new User;
$value = [
     [
         'id' => 1,
         'status' => 'active'
     ],
     [
         'id' => 5,
         'status' => 'deactive',
         'nickname' => 'digital'
     ],
     [
         'id' => 10,
         'status' => 'active',
         'date' => Carbon::now()
     ],
     [
         'id' => 11,
         'username' => 'splash'
     ]
];
$index = 'id';

Batch::update($userInstance, $value, $index);
```

# Example Increment / Decrement

```php
use App\Models\User;

$userInstance = new User;
$value = [
     [
         'id' => 1,
         'balance' => ['+', 500] // Add
     ] ,
     [
         'id' => 2,
         'balance' => ['-', 200] // Subtract
     ] ,
     [
         'id' => 3,
         'balance' => ['*', 5] // Multiply
     ] ,
     [
         'id' => 4,
         'balance' => ['/', 2] // Divide
     ] ,
     [
         'id' => 5,
         'balance' => ['%', 2] // Modulo
     ] ,
];
$index = 'id';

Batch::update($userInstance, $value, $index);
```

# Example Insert

```php
use App\Models\User;

$userInstance = new User;
$columns = [
     'firstName',
     'lastName',
     'email',
     'isActive',
     'status',
];
$values = [
     [
         'Mario',
         'Rawady',
         'rawadymario@dgsplash.com',
         '1',
         '0',
     ] ,
     [
         'Super',
         'Admin',
         'superadmin@dgsplash.com',
         '1',
         '0',
     ] ,
     [
         'Normal',
         'Admin',
         'normaladmin@dgsplash.com',
         '1',
         '0',
     ] ,
];
$batchSize = 500; // insert 500 (default), 100 minimum rows in one query

$result = Batch::insert($userInstance, $columns, $values, $batchSize);
```


```php
// result : false or array

sample array result:
Array
(
    [totalRows]  => 384
    [totalBatch] => 500
    [totalQuery] => 1
)
```

# Example called from model

Add `HasBatch` trait into model:

```php
namespace App\Models;

use DigitalSplash\Batch\Traits\HasBatch;

class User extends Model
{
    use HasBatch;
}
```

And call `batchUpdate()` or `batchInsert()` from model:

```php
use App\Models\User;

// ex: update
User::batchUpdate($value, $index);

// ex: insert
User::batchInsert($columns, $values, $batchSize);
```

# Helper batch()

```php
// ex: update

$result = batch()->update($userInstance, $value, $index);


// ex: insert

$result = batch()->insert($userInstance, $columns, $values, $batchSize);
```

# Tests
If you don't have phpunit installed on your project, first run `composer require phpunit/phpunit`

In the root of your laravel app, run `./vendor/bin/phpunit ./vendor/digital-splash/laravel-batch/tests`
