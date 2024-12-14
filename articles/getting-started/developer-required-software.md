<!-- Filename: J4.x:Developer:_Required_Software / Display title: Required Software -->

## Introduction

This tutorial is aimed at newcomers to Joomla code development who wish to prepare extensions on a local computer. It does not matter whether you use Windows, Mac or Linux. All of the required software is available for all of these platforms. To get started on your own laptop or desktop computer, which usually has the domain name *localhost*, you will need to install a standard set of separate items of software.

- **Apache**, a web server. Other web servers are available but newcomers should stick with Apache.
- **MySQL** or **MariaDB**, database servers. PostgreSQL is also supported but is not for newcomers.
- **PHP**, the latest Joomla recommended version or the minimum version for your platform.
- **phpMyAdmin** a tool used for managing MySQL and MariaDB databases.
- **xDebug** an extension to PHP used for debugging.
- An IDE such as **VSCode**. Others are available and covered in a separate article.

## Software Stacks

The first four items in this list are often referred to as a stack and may be named LAMP, MAMP or WAMP, where the letters in the acronym mean the following:

- **L, M or W** platform. L for Linux, M for Mac and W for Windows.
- **A: Apache** web server.
- **M: MySQL or MariaDB** database. The two are interchangeable.
- **P: PHP** scripting language. A widely used scripting language. There is no alternative as Joomla is coded in PHP.

## Packaged Stacks

A good way to get started is by using a package that combines the essential software:

- **WAMP** for Windows is free from the [Wampserver](https://www.wampserver.com/en/) site.
- **Bearsampp** for Windows is free from the [Bearsampp](https://bearsampp.com/) site. It has more tools.
- **XAMPP** For Windows, Mac and Linux is free from the [Apache Friends](https://www.apachefriends.org/) site. There is a local tutorial for [XAMPP](jdocmanual?article=user/hosting/local-hosting-with-xampp).
- **MAMP** for Mac and Windows comes in and free and commercial versions from the [MAMP](https://www.mamp.info/en/mac/) site.

## No Stacks

If you are have a Linux or Macintosh computer you will find that you can install each of the required software items independently from the remote repositories that support your OS. It is possible they may be installed and ready to go. To test, open your favourite web browser and enter **localhost** in the url bar. You will see a placeholder page or connection error page.

## Web Server Document Root Directory

On installation, your Apache server will have set a default document root directory. The location varies from platform to platform and you either need to know where it is or create a virtual host to put it where you want it. Example default locations:

- Mac OS: "/Library/WebServer/Documents"
- Linux: /var/www/html
- Windows: ...

To avoid later problems with file permissions it is often convenient to create a virtual host to point to the public_html directory of your own file space. That could be /home/username/public_html on Linux or /Users/username/Sites on Mac.

This is an example Mac virtual host entry in file /etc/apache2/vhosts/localhost.conf:

```bash
<VirtualHost *:80>
        DocumentRoot "/Users/username/Sites"
        ServerName localhost
        ErrorLog "/private/var/log/apache2/localhost-error_log"
        CustomLog "/private/var/log/apache2/localhost-access_log" common
        <Directory "/Users/username/Sites">
            AllowOverride All
            Require all granted
        </Directory>
</VirtualHost>
```

Alternatively, you may be able to create a symbolic link from the default document root to the public_html folder of your personal file space.
