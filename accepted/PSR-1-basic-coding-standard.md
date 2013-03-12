基础代码标准
=====================

本文将标准化一些标准编码元素，以确保高级技术间之PHP程式码的互通性。

在文件中所使用到的关键字“MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”,
“SHOULD NOT”, “RECOMMENDED”, “MAY”,
以及“ OPTIONAL”皆引用自[RFC 2199][]中说明。

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md


1. 概述
-----------

- 文件中一定(MUST)只能使用  `<?php` 和`<?=` 标签.

- PHP程序的文件编码，一定(MUST)要用“无BOM的UTF-8″格式。

- 文件需要(SHOULD)定义 (classes, functions, constants,等)，
- 或者遵从从属效应 (像是产生输出，变动.ini的设定等)，
- 不要(SHOULD NOT )在两种同时使用。

- 命名空间和类一定(MUST)  依循着 [PSR-0][]建议文件。

- 类名一定(MUST)采用大写字母开始的驼峰大小写命名法(StudlyCaps)  来定义。

- 类中的常量一定(MUST)是由全大写字母以及下划线符号组成。

- 函数名称一定(MUST)是以小写开始的驼峰大小写命名法(camelCase)定义。


2. 文件
--------

### 2.1. PHP 标签

PHP代码一定要(MUST)  使用长标签 <?php ?>  或是短输出标签 <?= ?>  ；
一定不能使用其它标签。

### 2.2. 字符编码

PHP代码一定要(MUST) 只使用无BOM的UTF-8编码。

### 2.3. 从属效应

一个文件中所需要(SHOULD)的，要么就是只定义新的symbols (类别、函式以及常数变数等)；
要么就是只采用从属效应(side effects)，就是不能(SHOULD NOT)二者都在同一个文件中出现。

“从属效应(Side effects)”一词系指在撰写程序逻辑时，完全只用到*包含进来的文件*中的类、函数或是常量等，而不再自行定义。

“从属效应(Side effects)”包含(但还不仅止于此)：产生输出、明确地使用 require  或 include  、
连接外部的服务、修改ini的设定、发出错误或例外、更动全局或静态变量、从文件读取或写入以及诸如此类的动作。

下面这个范例是应该要去避免的情形，一个档案中包含着定义以及从属效应：

```php
<?php
// side effect: change ini settings
ini_set('error_reporting', E_ALL);

// side effect: loads a file
include "file.php";

// side effect: generates output
echo "<html>\n";

// declaration
function foo()
{
    // function body
}
```

再来这个例子是示范一个文件中没有从属效应的定义方式：

```php
<?php
// declaration
function foo()
{
    // function body
}

// conditional declaration is *not* a side effect
if (! function_exists('bar')) {
    function bar()
    {
        // function body
    }
}
```


3. 命名空间与类名称
----------------------------

命名空间以及类别一定要(MUST)依循 [PSR-0][].

这表示每一个类在它自身的文件中，且它的命名空间至少要有一层：最上层的提供者名称。

类名一定要(MUST)用大写开始的驼峰大小写命名法(StudlyCaps)  定义。

PHP 5.3 以后的程序代码必需使用正式的命名空间。

举例来说：

```php
<?php
// PHP 5.3 以后要这么写：
namespace Vendor\Model;

class Foo
{
}
```

5.2.x以前的代码，命名类时需要(SHOULD)采用前缀为开头的类名之伪命名空间转化法。

```php
<?php
// PHP 5.2.x 及以前版本：
class Vendor_Model_Foo
{
}
```

4. 类的常量、属性以及方法
-------------------------------------------

术语“类”("class")指所有class, interface 和 trait.

### 4.1. 常量

类中定义的常量名称一定(MUST)要由大写字母以及下划线符号所组成。
举例来说：

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2. 属性

像这类的命名公约应该(SHOULD)是要用在一个合理的范围里，而这个范围应该是在：
提供者层级(vendor-level)、封包层级(package-level)、类别层级(class-level )或method-level (函式层)。

因此，本文件故意不对属性名称做像是 驼峰式命名($StudlyCaps、$camelCase)  或是 底线分隔($under_score)  这样的命名建议。

### 4.3. 方法

方法名一定(MUST) 是采用小写开始的驼峰大小写命名法(`camelCase()`)  定义。
