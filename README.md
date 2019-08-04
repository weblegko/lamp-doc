##Добавление нового домена в LAMP или создание виртуального хоста:

Создадим папку под домен:
sudo mkdir -p /var/www/example.com/public_html  

Сделаем папку своей собственостью нужными правами:  
sudo chown -R $USER:$USER /var/www/example.com/public_html  

Сделаем доступной папку:  
sudo chmod -R 755 /var/www  

Создадим файл для тестирования с каким нибудь контентом
nano /var/www/example.com/public_html/index.html   
  
Создаем файл виртуального хоста апапча (копируем дефолтный и правим):  
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.com.conf  
Редактируем:  
sudo nano /etc/apache2/sites-available/example.com.conf  
  
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
sudo a2ensite example.com.conf  
  
Отключить стандартный виртуальный хост 000-default.conf:  
sudo a2dissite 000-default.conf  

Перезапустить Apache2:  
sudo systemctl restart apache2  

Редактировать hosts:  
sudo nano /etc/hosts    
127.0.0.1   example.com  
  
Тестируем новый домен:  
http://example.com  
