# Apache-deployment
----------------------------------------
This Repo shows how to deploy a website using apache & Linux Ubuntu on a virtual machine.
To do that, follow these steps:
1. Install ubuntu on a virtual box (I used VMware). You can check this link:
https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox#1-overview
2. Install apache2 on that server.
    - First, open the terminal and run:
     ```shell
     sudo apt update
     sudo apt upgrade
     ```
    - Then, to install apache2, run the command:
     ```shell
     sudo apt install apache2
     ```
   - To test it out, find your ip using the command ```hostname -I``` and type your IP address on the web browser. This will show up:
   ![My Remote Image](https://i.stack.imgur.com/Cgr5L.png)
For more info, check:
https://ubuntu.com/tutorials/install-and-configure-apache#1-overview
3. Now to set up the static website and configure the subdomains:
   - Create the website files:
      - Create four HTML files for the website pages (I'm using a ready-website that contains these 4 pages, so I did not do this step)

           - index.html
           - home.html
           - about.html
           - contact.html
      - Place the files in their corresponding directories, run this command:
        ```shell
        sudo mv [nameOfFile] [DirectoryPath]
        ```

           - index.html in /var/www/html
           - home.html in /var/www/html/home
           - about.html in /var/www/html/about
           - contact.html in ~/contact
   -  Edit the hosts file:
      
      Open the /etc/hosts file using ```sudo nano /etc/hosts``` (you can use vim instead) and add the following lines at the end:

       - yourWebServerIpAddress www.[yourDoaminName].com
       - yourWebServerIpAddress www.about.[yourDoaminName].com
       - yourWebServerIpAddress www.home.example.[yourDoaminName].com
       - yourWebServerIpAddress www.contact.example.[yourDoaminName].com
           
      Replace "yourWebServerIpAddress" with the IP address of your web server and "[yourDoaminName]" with your first and last name (This is according to an assignment that was given to me)     
   - Rename the hostname:
   
        Open the /etc/hostname file using ```sudo nano /etc/hostname``` and replace the current hostname with your domain name.

   - Configure subdomains:
   
        Create a new virtual host file for each subdomain under /etc/apache2/sites-available:
        ```shell
       cd /etc/apache2/sites-available
       ```
       - about.[yourDoaminName].com.conf
       ```shell
       sudo nano about.[yourDoaminName].com.conf 
       ```
       ```shell
       <VirtualHost *:80>
           ServerName www.about.[yourDoaminName].com
           DocumentRoot /var/www/html/about
           <Directory /var/www/html/about>
               Options Indexes FollowSymLinks MultiViews
               AllowOverride All
               Require all granted
           </Directory>
       </VirtualHost>
       ```
       
       - home.example.[yourDoaminName].com.conf
       ```shell
       sudo nano home.example.[yourDoaminName].com.conf 
       ```
       ```shell
       <VirtualHost *:80>
           ServerName www.home.example.[yourDoaminName].com
           DocumentRoot /var/www/html/home
           <Directory /var/www/html/home>
               Options Indexes FollowSymLinks MultiViews
               AllowOverride All
               Require all granted
           </Directory>
       </VirtualHost>
       ```
       - contact.example.[yourDoaminName].com.conf 
        ```shell
       sudo nano contact.example.[yourDoaminName].com.conf 
       ```
       ```shell
       <VirtualHost *:80>
           ServerName www.contact.example.[yourDoaminName].com
           DocumentRoot /home/[yourUsername]/contact
           <Directory /home/[yourUsername]/contact>
               Options Indexes FollowSymLinks MultiViews
               AllowOverride All
               Require all granted
           </Directory>
       </VirtualHost>
       ```
   - Enable virtual hosts:
   
      Run the following command for each virtual host file to enable it:    
      ```shell
      sudo a2ensite [virtualHostName].conf
      ```
      Replace [virtualHostName] with the name of the virtual host file (e.g. about[yourDoaminName].com).
   -  Grant permissions:
   
      Run the following commands to grant the required permissions for the contact subdomain:
      ```shell
      sudo chmod -R 755 /home/[yourUsername]/contact
      sudo chown -R www-data:www-data /home/[yourUsername]/contact
      ```
   - Restart Apache:
   
      Run the following command to restart Apache:  
      ```shell
      sudo systemctl restart apache2
      ```
   - Open the /etc/hosts in windows in a notepad (make sure to run it as administartion), and add the same lines you added in the apache /etc/hosts.  
