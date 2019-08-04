# lampconf
## Натсраиваем виртуальные хосты в lamp

sudo mkdir -p /var/www/example.com/public_html  
sudo chown -R $USER:$USER /var/www/example.com/public_html  
sudo chmod -R 755 /var/www  
nano /var/www/example.com/public_html/index.html  
  
Создаем файл виртуального хоста:  
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.com.conf  
Редактируем:  
sudo nano /etc/apache2/sites-available/example.com.conf  
  
<VirtualHost *:80>  
    ServerAdmin admin@example.com   
    ServerName example.com  
    ServerAlias www.example.com  
    DocumentRoot /var/www/example.com/public_html  
    ErrorLog ${APACHE_LOG_DIR}/error.log  
    CustomLog ${APACHE_LOG_DIR}/access.log combined  
</VirtualHost>  

Включение виртуального хоста:  
sudo a2ensite example.com.conf  
  
Затем отключите стандартный виртуальный хост 000-default.conf:  
sudo a2dissite 000-default.conf  
  
Перезапустите Apache:  
sudo systemctl restart apache2  
  
sudo nano /etc/hosts  
127.0.0.1   example.com  
  
Тестируем в браузере:  
http://example.com  
