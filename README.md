# FastDLL-Pterodactyl v1.X
Very simple Fast DLL pterodactyl setup.
This suports any of HL1/HL2 games and mods such as CS:GO and TF2

# Instalation

Follow these 3 simple steps to install this module. 


## Step 1 (Apache or NGINX Configuration ):

### Apache:
  1. Copy your apache file "FastDLL-Apache.conf" or "FastDLL-ApacheSSL.conf" in etc/apache2/sites-available or /etc/httpd/conf.d/ (If on CentOS)
  2. Edit the DNS.DOMANIN.COM from the file to your Fully Qualified Domain Name (FQDN), you can find it in your node settings.

IF YOU TRY USING SSL
When using the SSL configuration you MUST create SSL certificates, otherwise your webserver will fail to start. Pterodacyl has a doc about this you can find it [HERE](https://pterodactyl.io/tutorials/creating_ssl_certificates.html#method-1:-certbot) .You need to certificate the **FQDN** in order to work !!!!

### NGINX:

  1. Copy your NGINX file "FastDLL-NGINX.conf" to conf.d directory of a nginx daemon.
 *  Edit : ``` server_name  DNS.DOMANIN.COM; to your Fully Qualified Domain Name (FQDN); ```

## Step 2 (SSH):
1. The following command assings the right permission for the module and to assign a user www.data for pterodactyl
  * ``` gpasswd -a www-data pterodactyl && chmod 755 /var/lib/pterodactyl/ && chmod 755 /var/lib/pterodactyl/volumes/ ```
2. Restart web configuration:
  ##### NGINX:
      systemctl restart nginx or nginx -s reload
  ##### Apache:
    * You do not need to run any of these commands on CentOS (For CentOS systemctl restart httpd ) *
    sudo ln -s /etc/apache2/sites-available/FastDLL-Apache.conf /etc/apache2/sites-enabled/FastDLL-Apache.conf  (For Apache NON SSL)
    sudo ln -s /etc/apache2/sites-available/FastDLL-ApacheSSL.conf /etc/apache2/sites-enabled/FastDLL-ApacheSSL.conf (For Apache WITH SSL)
    sudo a2enmod rewrite
    systemctl restart apache2
 ## Step 3 (Pannel edit):
 1. Edit the eggs install script with the following line at the very bottom end:
 *  ```  chmod 750 /mnt/server/ ```.
   If you allready have installed a server and want to add FastDLL you need to run this command in SSH:
 * ``` chmod 755 /var/lib/pterodactyl/volumes/SERVER_UUID ``` Change the UUID to yours
 2. Configure our egg to use a JSON file parser:
 Edit the Configuration Files section from the egg config with the following: (For CSGO)
 ```
 {
    "csgo/cfg/server.cfg": {
        "parser": "file",
        "find": {
            "sv_downloadurl": "sv_downloadurl \"http://{{env.P_SERVER_LOCATION}}/{{env.P_SERVER_UUID}}/csgo/\"",
            "sv_allowupload": "sv_allowupload \"0\""
            "sv_allowdownload": "sv_allowdownload \"1\"",
        }
    }
}
```
![alt text](https://i.imgur.com/4exzabq.png)
3. Name your node locations as our **FQDN** so it will act as a link or if you use only one node and onle location you can manualy edit the code and change {{env.P_SERVER_LOCATION}} in to your **FQDN** for example: 
  *   ```"sv_downloadurl": "sv_downloadurl \"http://DNS.DOMAIN.COM/{{env.P_SERVER_UUID}}/csgo/\"",```
