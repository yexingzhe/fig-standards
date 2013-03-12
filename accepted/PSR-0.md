如下描述为编写autoloader时提供遵循必须条件。

必要条件
---------

* 合格的命名空间(namespace) 和 类(class) 必须采用如此结构 `\<Vendor Name>\(<Namespace>\)*<Class Name>`
* 每个命名空间都需要有一个顶层的命名空间 ("Vendor Name").
* 每个命名空间可以有多个子命名空间.
* 如果命名空间是从文件系统载入时，分割符转化成`DIRECTORY_SEPARATOR` .
* 类名中的下划线符号`_` 转化成
  `DIRECTORY_SEPARATOR`. 因为命名空间中`_` 没有意义.
* 从文件系统载入的合格命名空间及类须以`.php` 结尾.
* 外部库名、命名空间名、类名的单词可以任意大小写混合.

范例
--------

* `\Doctrine\Common\IsolatedClassLoader` => `/path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php`
* `\Symfony\Core\Request` => `/path/to/project/lib/vendor/Symfony/Core/Request.php`
* `\Zend\Acl` => `/path/to/project/lib/vendor/Zend/Acl.php`
* `\Zend\Mail\Message` => `/path/to/project/lib/vendor/Zend/Mail/Message.php`

命名空间和类名中的下划线
-----------------------------------------

* `\namespace\package\Class_Name` => `/path/to/project/lib/vendor/namespace/package/Class/Name.php`
* `\namespace\package_name\Class_Name` => `/path/to/project/lib/vendor/namespace/package_name/Class/Name.php`

这个标准是我们一般使用autoloader的最低标准。你可以参考下面的
`SplClassLoader`
实例以此标准载入PHP 5.3的类库.

实例
----------------------

下面是一个解释如何实践以上标准的实例
```php
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
```

SplClassLoader 实例
-----------------------------

下面的 gist 是一个 SplClassLoader 实例，你可以参考上面的autoloader命名标准载入自己的类库. 它是目前我们强力建议的载入 PHP
5.3 类库的标准.

* [http://gist.github.com/221634](http://gist.github.com/221634)

