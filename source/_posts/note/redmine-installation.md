---
title: Redmine 项目管理 - 手动安装
abbrlink: f39f15a2
categories:
  - - 笔记
tags:
  - redmine
  - 项目管理
date: 2019-04-15 10:24:05
---

## 环境

- 系统 Ubuntu18.04 Server LTS

- 系统没有安装 Apache 或 MySQL

## 安装

1. 安装APACHE2

  ```sh
  sudo apt update
  sudo apt install apache2 libapache2-mod-passenger

  sudo systemctl stop apache2.service
  sudo systemctl start apache2.service
  sudo systemctl enable apache2.service
  ```

1. 安装数据库MYSQL

  ```sh
  sudo apt-get install mysql-server

  sudo systemctl stop mysql.service
  sudo systemctl start mysql.service
  sudo systemctl enable mysql.service
  ```

  运行以下命令以保护MYSQL服务器的安全：

  ```sh
  sudo mysql_secure_installation         

  New password: (输入密码)
  Re-enter new password: (重复输入)
  Remove anonymous users? [Y/n]: Y
  Disallow root login remotely? [Y/n]: Y
  Remove test database and access to it? [Y/n]:  Y
  Reload privilege tables now? [Y/n]:  Y
  ```

1. 创建 REDMINE 数据库

  ```sh
  sudo mysql -u root -p

  mysql> create database redmine;
  mysql> create user 'redmine'@'localhost' identified by '123456';
  mysql> grant all on redmine.* to 'redmine'@'localhost' identified by '123456' with grant option;
  mysql> flush privileges;
  mysql> exit;
  ```

  创建用户或授权时有可能报如下错误：

  ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
  这是由于密码太简单，不满足安全策略造成的。查看MYSQL初始的密码策略：

  ```sh
  mysql> show variables like 'validate_password%';
  +--------------------------------------+-------+
  | Variable_name                        | Value |
  +--------------------------------------+-------+
  | validate_password_check_user_name    | OFF   |
  | validate_password_dictionary_file    |       |
  | validate_password_length             | 6     |
  | validate_password_mixed_case_count   | 1     |
  | validate_password_number_count       | 1     |
  | validate_password_policy             | LOW   |
  | validate_password_special_char_count | 1     |
  +--------------------------------------+-------+
  7 rows in set (0.41 sec)
  ```

  解决办法：1. 设置更复杂的密码，2. 降低安全策略：

  ```sh
  set global validate_password_policy=LOW;
  set global validate_password_length=6;
  ```

1. 安装REDMINE

  ```sh
  sudo apt-get install redmine redmine-mysql
  ```

  ![图片](/images/npm-package-publish-01.webp)

  ![图片](/images/npm-package-publish-02.webp)

  ![图片](/images/npm-package-publish-03.webp)

  ![图片](/images/npm-package-publish-04.webp)

  在安装过程中，系统将要求配置REDMINE，选择YES，然后继续。数据库选择MYSQL；接下来为REMIND实例创建一个密码以在数据库中注册，密码为上面创建的数据库用户的密码；接下来，安装GEM、BLUNDLER软件包。

  ```sh
  sudo gem update
  sudo gem install bundler
  ```

  更新过程中会报如下错误，不过暂时可以忽略。

  ```sh
  Fetching: mysql2-0.5.3.gem (100%)
  Building native extensions. This could take a while...
  ERROR:  Error installing mysql2:
      ERROR: Failed to build gem native extension.

      current directory: /var/lib/gems/2.5.0/gems/mysql2-0.5.3/ext/mysql2
  /usr/bin/ruby2.5 -r ./siteconf20200414-8181-7u476x.rb extconf.rb
  mkmf.rb can't find header files for ruby at /usr/lib/ruby/include/ruby.h

  extconf failed, exit code 1

  Gem files will remain installed in /var/lib/gems/2.5.0/gems/mysql2-0.5.3 for inspection.
  Results logged to /var/lib/gems/2.5.0/extensions/x86_64-linux/2.5.0/mysql2-0.5.3/gem_make.out
  ```

1. 为REDMINE设置APACHE2站点。首先运行以下命令创建指向REDMINE文档根目录的软链接：

  ```sh
  sudo ln -s /usr/share/redmine/public /var/www/html/redmine
  ```

1. 配置 APACHE2

  运行以下命令以打开PASSENGER.CONF文件：

  ```sh
  sudo vim /etc/apache2/mods-available/passenger.conf
  ```

  然后将突出显示的行添加到文件中并保存：

  ```conf
  <IfModule mod_passenger.c>
      PassengerDefaultUser www-data
      PassengerRoot /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini
      PassengerDefaultRuby /usr/bin/ruby
  </IfModule>
  ```

  为REDMINE配置APACHE2站点配置文件。该文件将控制用户访问REDMINE内容的方式。运行以下命令以创建一个名为REDMINE.CONF的新配置文件：

  ```sh
  sudo vim /etc/apache2/sites-available/redmine.conf
  ```

  ```conf
  <VirtualHost *:80>
      ServerAdmin redmime@demo.com
      DocumentRoot /var/www/html/redmine
      ServerName demo.com
      ServerAlias redmine.demo.com

      <Directory /var/www/html/redmine>
          RailsBaseURI /redmine
          PassengerResolveSymlinksInDocumentRoot on
      </Directory>

      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined

  </VirtualHost>
  ```

1. 启用REDMINE站点

  ```sh
  sudo a2ensite redmine.conf
  sudo systemctl restart apache2.service
  ```

  访问页面：http://redmine.demo.com/

  ![图片](/images/npm-package-publish-05.webp)

  登录账户密码：admin/admin

  ![图片](/images/npm-package-publish-06.webp)

  首次登陆要求修改密码
