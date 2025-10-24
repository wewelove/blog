---
title: Yii2 多语言配置 - i18n 国际化
date: 2017-12-05 11:36:14
categories:
  - 笔记
tags:
  - php
  - yii2
  - i18n
---

## 原理

国际化 `i18n` 是 `Yii2` 的核心组件，应用 `bootstrap` 时 `i18n` 就会被实例化。

```php
// yii2/base/Application.php
public function coreComponents()
{
    return [
        'log' => ['class' => 'yii\log\Dispatcher'],
        'view' => ['class' => 'yii\web\View'],
        'formatter' => ['class' => 'yii\i18n\Formatter'],
        'i18n' => ['class' => 'yii\i18n\I18N'],
        'mailer' => ['class' => 'yii\swiftmailer\Mailer'],
        'urlManager' => ['class' => 'yii\web\UrlManager'],
        'assetManager' => ['class' => 'yii\web\AssetManager'],
        'security' => ['class' => 'yii\base\Security'],
    ];
}
```

默认情况 `i18n` 会把实现交给 `yii\i18n\PhpMessageSource` 类，这也是项目中最常用的方式。在不修改应用配置的情况下，只需要将多语言的 php 文件放到 `@app/messages` 目录下即可。如 `zh-CN` 中文配置 `@app/messages/zh-CN/app.php`，文件名必须为 `app.php`。
  
除此外还有 [`yii\i18n\GettextMessageSource`](http://www.yiichina.com/doc/api/2.0/yii-i18n-dbmessagesource) 和 [`yii\i18n\DbMessageSource`](http://www.yiichina.com/doc/api/2.0/yii-i18n-gettextmessagesource) 可用。

```php
// yii2/i18n/I18N.php
public function init()
{
    parent::init();
    if (!isset($this->translations['yii']) && !isset($this->translations['yii*'])) {
        $this->translations['yii'] = [
            'class' => 'yii\i18n\PhpMessageSource',
            'sourceLanguage' => 'en-US',
            'basePath' => '@yii/messages',
        ];
    }

    if (!isset($this->translations['app']) && !isset($this->translations['app*'])) {
        $this->translations['app'] = [
            'class' => 'yii\i18n\PhpMessageSource',
            'sourceLanguage' => Yii::$app->sourceLanguage,
            'basePath' => '@app/messages',
        ];
    }
}
```

如何配置，如何使用？关键要理解 `i18n` 是如何根据配置文件加载多语言文件的。

```php
// yii2/i18n/I18N.php
public function getMessageSource($category)
{
    // 判断 $category 在配置中是否存在
    // 1. 先进行完全匹配
    // 2. 再匹配结尾带 * 的配置
    // 3. 最后直接引用 * 配置
    // 如果某步成功，就把任务交给 PhpMessageSource，
    // 并将 $category 传递给 PhpMessageSource
}

// yii2/i18n/PhpMessageSource.php
protected function getMessageFilePath($category, $language)
{
    // PhpMessageSource 关键任务是找到多语言文件的路径
    // 根据 $category 获取多语言文件的路径
    // 1. 先从 fileMap 配置中获取路径
    // 2. 再用 $category 拼接路径
    // loadMessages() 方法根据路径加载多语言数据返回 I18N
}
```

## 使用

1. 简单配置

    配置文件如下：

    ```php
    'i18n' => [ 
        'translations' => [ 
            'demo' => [ 
                'class' => 'yii\i18n\PhpMessageSource', 
                'basePath' => '@app/messages',
            ],
        ],
    ], 
    ```

    文件存放位置：

    ```php
    @app/messages/zh-CN/demo.php
    ```

    引用：

    ```php
    Yii::t('demo', 'key');
    ```

2. 分层级，通过文件目录实现

    配置文件如下：

    ```php
    'i18n' => [ 
        'translations' => [ 
            'demo*' => [ 
                'class' => 'yii\i18n\PhpMessageSource', 
                'basePath' => '@app/messages',
            ],
        ],
    ], 
    ```

    文件存放位置：

    ```php
    @app/messages/zh-CN/demo/common.php
    ```

    引用：

    ```php
    Yii::t('demo/common', 'key');
    ```

3. 分层级，通过配置实现

    配置文件如下：

    ```php
    'i18n' => [ 
        'translations' => [ 
            'demo*' => [ 
                'class' => 'yii\i18n\PhpMessageSource', 
                'basePath' => '@app/messages',
                'fileMap' => [
                    // 文件名应该与键名对应，即 common.php
                    // 此处只是为了说明问题
                    'demo/common' => 'main.php'
                ]
            ],
        ],
    ], 
    ```

    文件存放位置：

    ```php
    @app/messages/zh-CN/main.php
    ```

    引用：

    ```php
    Yii::t('demo/common', 'key');
    ```

4. 模块中独立配置

    在模块入口文件 Module.php 中，增加如下代码

    ```php
    public function init()
    {
        parent::init();

        if ( !isset(Yii::$app->get('i18n')->translations['demo*']) ) {
            Yii::$app->get('i18n')->translations['demo*'] = [
                'class' => 'yii\i18n\PhpMessageSource', 
                'basePath' => __DIR__ . '/messages', 
                'fileMap' => [
                    'demo/common' => 'common.php',
                ],
            ];
        }
    }
    ```

    多语言文件放在模块的 `./messages/` 目录下。

    引用：

    ```php
    Yii::t('demo/common', 'key');
    ```
