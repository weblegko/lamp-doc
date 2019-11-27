## Установка LAMP на Ubuntu 18.04
[Ссылка на статью](https://losst.ru/ustanovka-lamp-ubuntu-18-04)

*sudo apt install apache2*
*sudo apt install mysql-server*
*sudo apt install php7.2 libapache2-mod-php7.2 php-mysql*
*sudo apt install php-curl php-json php-cgi php-gd php-zip php-mbstring php-xml php-xmlrpc*

### Настройка брандмауэра

*sudo ufw allow in 80/tcp*

### Проверка работы LAMP

*sudo nano /var/www/html/phpinfo.php*
```
<?php phpinfo(); ?>
```
Тестируем по ссылке [http://localhost/phpinfo.php](http://localhost/phpinfo.php)


## Установка phpmyadmin
[Ссылка на статью](https://losst.ru/ustanovka-phpmyadmin-ubuntu-18-04)
 
 
*sudo apt install phpmyadmin*  
  
*sudo ln -s /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf*  
*sudo a2enconf phpmyadmin*  
*sudo service apache2 reload*  

### Создание пользователя для phpmyadmin
  
*sudo mysql*  
```
> CREATE USER 'admin'@'localhost' IDENTIFIED BY 'пароль';  
> GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;  
> FLUSH PRIVILEGES;  
```

## Добавление нового домена в LAMP - создание виртуального хоста

Создадим папку под домен:  
*sudo mkdir -p /var/www/example.com/public_html*  

Сделаем папку своей собственостью нужными правами:  
*sudo chown -R $USER:$USER /var/www/example.com/public_html*  

Сделаем доступной папку:  
*sudo chmod -R 755 /var/www*  

Создадим файл для тестирования с каким нибудь контентом  
*nano /var/www/example.com/public_html/index.html*   
  
Создаем файл виртуального хоста апапча (копируем дефолтный и правим):  
*sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.com.conf*  

Редактируем:  
*sudo nano /etc/apache2/sites-available/example.com.conf*  
  
Добавим запись своего домена:  
```apache  
<VirtualHost *:80>  
    ServerAdmin admin@example.com  
    ServerName example.com  
    ServerAlias www.example.com  
    DocumentRoot /var/www/example.com/public_html  
    ErrorLog ${APACHE_LOG_DIR}/error.log  
    CustomLog ${APACHE_LOG_DIR}/access.log combined  
</VirtualHost>  
```
  
Включаем виртуальныйо хост:  
*sudo a2ensite example.com.conf*  

Для запуска всех доступных виртуальных хостов:

cd /etc/apache2/sites-available  
sudo a2ensite *   

  
Отключить, если нужно стандартный виртуальный хост 000-default.conf:  
*sudo a2dissite 000-default.conf*  

Перезапустить Apache2:  
*sudo systemctl restart apache2*  

Редактировать hosts:  
*sudo nano /etc/hosts*  
```
127.0.0.1   example.com  
```

Тестируем новый домен:  
*http://example.com*  
