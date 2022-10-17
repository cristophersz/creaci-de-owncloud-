# Instal·lació Owncloud

sudo sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/apache2/apache2.conf

![Captura de pantalla de 2022-10-03 17-34-30](https://user-images.githubusercontent.com/116022089/196237713-2396d672-43e3-4f4c-9e04-428e05c5153d.png)


Després haurem d’instal·lar MariaDB:

sudo apt-get install mariadb-server mariadb-client -y

![Captura de pantalla de 2022-10-03 18-46-13](https://user-images.githubusercontent.com/116022089/196238035-61ab7949-3ff5-4dab-8e27-3adda4008146.png)


Un cop instal·lat haurem de configurar la instal·lació:

sudo mysql_secure_installation

![Captura de pantalla de 2022-10-03 18-48-53](https://user-images.githubusercontent.com/116022089/196238844-2a9033b3-911c-462a-b193-ecf3db938ce8.png)

![Captura de pantalla de 2022-10-03 18-49-17](https://user-images.githubusercontent.com/116022089/196239115-37b54fc1-12c3-439d-a8b1-ca2f7de2a9d8.png)

![Captura de pantalla de 2022-10-03 18-49-58](https://user-images.githubusercontent.com/116022089/196239145-cf84081c-7f41-4443-95aa-4a320bc80c41.png)

![Captura de pantalla de 2022-10-03 18-52-39](https://user-images.githubusercontent.com/116022089/196239162-2678fe6e-6ca3-41b9-9bf8-0868033fa62b.png)

![Captura de pantalla de 2022-10-03 18-53-57](https://user-images.githubusercontent.com/116022089/196239175-d54c69d9-06a4-429f-88bf-363750f8357e.png)


5.  Ja tenim MariaDB configurat ara haurem de reiniciar el servidor:

sudo systemctl restart mariadb.service` o `sudo service mariadb.service restart

![Captura de pantalla de 2022-10-03 19-08-05](https://user-images.githubusercontent.com/116022089/196239638-9ba4b9d2-6e4f-4c27-b63d-fc78af340894.png)

6. A continuació crearem la base de dades de Owncloud, entrem a Maridb:

sudo mysql -u root -p

![Captura de pantalla de 2022-10-03 19-11-38](https://user-images.githubusercontent.com/116022089/196239740-065d274e-fe04-4210-9ba9-04c2f5dbf8dd.png)

7. Crearem la base de dades: 

CREATE DATABASE owncloud;

![Captura de pantalla de 2022-10-03 19-13-03](https://user-images.githubusercontent.com/116022089/196239872-f4027436-8f2e-48a0-8cdb-c7b2a7133531.png)

8. Creem un usuari anomenat ownclouduser amb una contrasenya que podría ser Admin1234.
CREATE USER 'ownclouduser'@'localhost' IDENTIFIED BY 'Admin1234';

![Captura de pantalla de 2022-10-03 19-15-58](https://user-images.githubusercontent.com/116022089/196239917-b090f6d2-3f77-4340-8543-861435dbd15f.png)

9. Ara li donarem accés al usuari de la base de dades creada:
GRANT ALL ON owncloud.* TO 'ownclouduser'@'localhost' IDENTIFIED BY 'Admin1234' WITH GRANT OPTION;

10. Per últim aplicarem els canvis i sortirem:ç
 FLUSH PRIVILEGES;
EXIT;

![Captura de pantalla de 2022-10-03 19-21-05](https://user-images.githubusercontent.com/116022089/196240078-1780bb65-38ff-45f2-bbd5-eacbf3d1372f.png)

11. Ara instal·lem el PHP i els seus mòduls necessaris:
sudo apt-get install software-properties-common -y

![Captura de pantalla de 2022-10-03 19-25-57](https://user-images.githubusercontent.com/116022089/196240390-947b6054-d647-4d45-b1aa-27a91a03123b.png)

sudo add-apt-repository ppa:ondrej/php

![Captura de pantalla de 2022-10-03 19-28-50](https://user-images.githubusercontent.com/116022089/196240545-c0aa423d-bc74-492b-8d94-649e7f1fb7b0.png)

12. Actualitzem els paquets amb els repositoris afegits:

sudo apt update

![Captura de pantalla de 2022-10-04 17-31-05](https://user-images.githubusercontent.com/116022089/196240665-44c6251b-bfae-4e91-b0da-3a7b0d4b4f0f.png)

13. Hem de tindre en compte els requisits de Owncloud abans d'instal·lar els mòduls:
 sudo apt install php7.4 libapache2-mod-php7.4 php7.4-common php7.4-mbstring php7.4-xmlrpc php7.4-soap php7.4-apcu php7.4-smbclient php7.4-ldap php7.4-redis php7.4-gd php7.4-xml php7.4-intl php7.4-json php7.4-imagick php7.4-mysql php7.4-cli php7.4-mcrypt php7.4-ldap php7.4-zip php7.4-curl -y

![Captura de pantalla de 2022-10-04 17-39-53](https://user-images.githubusercontent.com/116022089/196240719-55482daf-7ddf-4eba-8d29-a984a0f18f31.png)

14. Després de la instal·lació editarem el fitxer php.ini i canviarem alguns valors:

sudo nano /etc/php/7.4/apache2/php.ini

Els valor que s’han de camnviar són aquests:

file_uploads = On

![Captura de pantalla de 2022-10-04 17-48-50](https://user-images.githubusercontent.com/116022089/196241008-d066093f-5342-47b4-98d1-67d48dfde01c.png)


allow_url_fopen = On

![Captura de pantalla de 2022-10-04 17-49-39](https://user-images.githubusercontent.com/116022089/196241130-81e45dcb-91c8-4add-bc65-bae644d771dc.png)

memory_limit = 256M

![Captura de pantalla de 2022-10-04 17-50-32](https://user-images.githubusercontent.com/116022089/196241202-7d49ec01-5692-4ba8-b882-44144f584e05.png)

upload_max_filesize = 100M

![Captura de pantalla de 2022-10-04 17-51-22](https://user-images.githubusercontent.com/116022089/196241232-a84a7bdd-611d-4324-92c8-747f785b4c1c.png)

display_errors = Off

![Captura de pantalla de 2022-10-04 17-52-05](https://user-images.githubusercontent.com/116022089/196241258-aab65931-eff9-4372-843d-22458016de19.png)

date.timezone = Europe/Madrid

![Captura de pantalla de 2022-10-04 17-52-56](https://user-images.githubusercontent.com/116022089/196241287-9d682753-fbe8-4811-80e9-3578951e9408.png)

15. Instal·larem Owncloud 
cd /tmp && wget https://download.owncloud.com/server/stable/owncloud-complete-latest.zip
unzip owncloud-complete-latest.zip
sudo mv owncloud /var/www/html/owncloud/

![Captura de pantalla de 2022-10-05 16-25-55](https://user-images.githubusercontent.com/116022089/196241473-4830396d-15f2-4b36-87ec-4abb2f2d7059.png)

16. Ara canviarem de propietari i de permisos del directori se Owncloud 

sudo chown -R www-data:www-data /var/www/html/owncloud/
sudo chmod -R 755 /var/www/html/owncloud/

![Captura de pantalla de 2022-10-05 16-36-01](https://user-images.githubusercontent.com/116022089/196241407-44718a30-20ae-4965-8361-6a6c0d6b5253.png)

17. Després configurarem Apache:

sudo nano /etc/apache2/sites-available/owncloud.conf
18.Haurem de modificar el fitxer i agregar aquesta nota dins del fitxer:

![Captura de pantalla de 2022-10-05 17-02-09](https://user-images.githubusercontent.com/116022089/196241570-9dc9548b-3b5a-4c20-8b9a-b3be84846180.png)

19. Habilitarem Owncloud i el modul rewrite:

sudo a2ensite owncloud.conf
sudo a2enmod rewrite
sudo a2enmod headers
sudo a2enmod env
sudo a2enmod dir
sudo a2enmod mime

![Captura de pantalla de 2022-10-05 17-09-40](https://user-images.githubusercontent.com/116022089/196241724-e374cb9f-5b3c-4a2c-af11-e544ab562364.png)

20. Reiniciem Apache:

sudo service Apache2 restart	

![Captura de pantalla de 2022-10-05 19-13-13](https://user-images.githubusercontent.com/116022089/196241750-08368f3e-6de5-461e-9751-993b1f58c6a1.png)



![Captura de pantalla de 2022-10-17 19-20-45](https://user-images.githubusercontent.com/116022089/196242259-fce78b51-d8a8-49d1-a0a6-ad55e7dc5bf7.png)
