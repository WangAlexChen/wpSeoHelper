# 如何构建 composer 包

## 背景

本人使用伪代码，分享如何从0开始构建一个 composer 包，假设我们现在希望开发一个简单的数据库操作组件，
以 composer 方式管理，并且将包发布到公开 composer 仓库 https://packagist.org/ 当中。

先创建一个目录，存放 composer 源代码：

```sh
$ mkdir wp-simple-database
$ cd wp-simple-database
```

## 一、Composer 包初始化

```sh
$ composer init
```

```sh
// 1. 输入命名空间及项目名称
                                            
  Welcome to the Composer config generator  

This command will guide you through creating your composer.json config.

Package name (<vendor>/<name>) [root/wp-simple-database]: mrzkit/wp-simple-database

// 2. 项目描述
Description []: 这是一个测试

// 3. 输入作者信息
shijie.zhu <kitgor.architect@gmail.com>

// 4. 输入最低稳定版本，例如: stable, release, beta, alpha, dev
Minimum Stability []: dev

// 5. 输入项目类型
Package Type (e.g. library, project, metapackage, composer-plugin) []: library

// 6. 输入授权类型, 例如: MIT, GPL
License []: GPL-3.0-or-later

// 7. 输入依赖信息
Define your dependencies.

Would you like to define your dependencies (require) interactively [yes]? 

// 如果需要依赖，则输入要安装的依赖
Search for a package: php

// 输入版本号
Enter the version constraint to require (or leave blank to use the latest version): >=5.4.0

// 如需多个，则重复以上两个步骤

// 8. 是否需要 require-dev
Would you like to define your dev dependencies (require-dev) interactively [yes]?

// 9. 自动加载映射
Add PSR-4 autoload mapping? Maps namespace "Mrzkit\WpSimpleDatabase" to the entered relative path. [src/, n to skip]: src/

// 10. 最后确认
{
    "name": "mrzkit/wp-simple-database",
    "description": "wp simple database",
    "type": "library",
    "license": "GPL",
    "autoload": {
        "psr-4": {
            "Mrzkit\\WpSimpleDatabase\\": "src/"
        }
    },
    "authors": [
        {
            "name": "shijie.zhu",
            "email": "kitgor.architect@gmail.com"
        }
    ],
    "minimum-stability": "dev",
    "require": {}
}

Do you confirm generation [yes]? 
```

* 关于许可说明清单

https://spdx.org/licenses/

## 二、伪代码

此时，初始化操作已经完成，然后，我们将源代码放入 src/ 目录中，如 `src/DatabaseManager.php`:

```php
namespace Mrzkit\WpSimpleDatabase;

class DatabaseManager {

    public function select() {

    }
    public function insert() {

    }
    public function update() {

    }
    public function delete() {

    }
}
```

上述代码就是我们要发布的全部源码。


## 三、创建仓库

登陆 github，进入 repositories 个人仓库页，创建一个新的代码仓库，Create a new repository。

1. 创建新仓库，完成初始化
```sh
# …or create a new repository on the command line
echo "# wp-simple-database" >> README.md
$ git init
$ git add README.md
$ git commit -m "first commit"
$ git branch -M main
$ git remote add origin git@github.com:Kit-Mrz/wp-simple-database.git
$ git push -u origin main
```

2. 如果是已经有的仓库，只需要换地址即可
```sh
# …or push an existing repository from the command line
$ git remote add origin git@github.com:Kit-Mrz/wp-simple-database.git
$ git branch -M main
$ git push -u origin main
```

## 四、打标签

```sh
$ git tag -a v1.0.0 -m "v1.0.0"

$ git push --tag origin main
```

## 五、发布 Composer 包

1. 登陆 PHP Composer 仓库

地址: https://packagist.org/

2. 验证 Composer 包

```sh
$ composer validate 
```

3. 提交代码仓库地址, Submit package

地址: https://packagist.org/packages/submit

将 github 的代码主页粘贴进去，例如：https://github.com/Kit-Mrz/simple-database

## 六、自动更新 GitHub Hook

启用 Packagist 服务钩子可以确保当您推到 GitHub 时，您的软件包总是会立即得到更新。

这里的基本原理就是，当你 PUSH 代码到 GitHub 时，GitHub 会触发一个 webhook，即向 packagist 发送一个请求。

- 查看 API token

https://packagist.org/profile/

点击 `Your API Token`，可以看到 TOKEN

- WebHook

在你的项目主页，如 https://github.com/Kit-Mrz/simple-database , 点击 Settings 进入设置，找到左则的 Webhooks 菜单，即可配置。

1. https://github.com/Kit-Mrz/simple-database

2. Settings

3. Webhooks

4. Add webhook

5. 将 API token 粘贴到 Secret 选项，保存即可

## 参考资料

- https://packagist.org/
- https://packagist.org/about#how-to-update-packages
- https://blog.programster.org/creating-a-github-webhook-for-packagist
