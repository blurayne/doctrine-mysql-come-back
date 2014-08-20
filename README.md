# DoctrineMySQLComeBack

Auto reconnect on Doctrine MySql has gone away exceptions on doctrine/dbal 2.3.x.

Tested with doctrine/dbal >=2.3,<2.4-dev, but could works with other versions.

# Installation

```console
$ composer require facile-it/doctrine-mysql-come-back dev-master
```

# Configuration

In order to use DoctrineMySQLComeBack you have to set `wrapperClass` and `driverClass` connection params.
You can choose how many times Doctrine should be able to reconnect, setting `x_reconnect_attempts` connection option. Its value should be an int.

An example of configuration at connection instantiation time:

```php
use Doctrine\DBAL\Configuration;
use Doctrine\DBAL\DriverManager;

$config = new Configuration();

//..

$connectionParams = array(
    'dbname' => 'mydb',
    'user' => 'user',
    'password' => 'secret',
    'host' => 'localhost',
    // [doctrine-mysql-come-back] settings
    'wrapperClass' => 'Facile\DoctrineMySQLComeBack\Doctrine\DBAL\Connection',
    'driverClass' => 'Facile\DoctrineMySQLComeBack\Doctrine\DBAL\Driver\PDOMySql\Driver',
    'x_reconnect_attempts' => 3
);

$conn = DriverManager::getConnection($connectionParams, $config);

//..
```

An example of yaml configuration on Symfony 2 projects:

```yaml
# Doctrine example Configuration
doctrine:
    dbal:
        default_connection: %connection_name%
        connections:
            %connection_name%:
                host:     %database_host%
                port:     %database_port%
                dbname:   %database_name%
                user:     %database_user%
                password: %database_password%
                charset:  UTF8
                wrapper_class: 'Facile\DoctrineMySQLComeBack\Doctrine\DBAL\Connection'
                driver_class: 'Facile\DoctrineMySQLComeBack\Doctrine\DBAL\Driver\PDOMySql\Driver'
                options:
                    x_reconnect_attempts: 3
```

Thanks to Dieter Peeters and his proposal on [DBAL-275](http://www.doctrine-project.org/jira/browse/DBAL-275). Check it out if you are aon doctrine/dbal 2.1.x.