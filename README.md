# Linux-Server-Configuration
Linux Server Configuration Project for Udacity's FSND

This project is meant to satisfy the project Rubric defined here:  https://review.udacity.com/#!/rubrics/7/view

pw for user grader='grader'

passphrase for key 'linuxProject' = 'austerity'

pw for user catalog='catalog'

In addition to references noted below, these discussion forums were very helpful:  https://discussions.udacity.com/t/tips-for-configuring-postgresql-and-setting-up-item-catalog-project/223436/13 and https://discussions.udacity.com/t/installing-postgres/246850/9

Here are the steps Taken
1.  Created Instance AWS/Lightsail.

    a.  Selected Unbuntu as instance image

    b.  Configured firewall in instance
    
        Application	Protocol	Port range	
    
        SSH	        TCP	      22	
        
        HTTP	        TCP	      80	
        
        Custom	        UDP	      123	
        
        Custom	        TCP	      2200	
  
    c.  Established static IP 52.43.165.216

2.  Updated Packages and added user 'grader'  See refernce notes found here:  http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/managing-users.html

    a.   - sudo apt-get update
    
    b.   -sudo apt-get upgrade
    
    c.   -sudo apt-get autoremove
    
    d.   -sudo apt-get install finger
    
    e.   -sudo adduser grader
                -set pw
                
    f.   -sudo su - grader   
    
    g.   -sudo visudo and added grader to ALL=(ALL: ALL) ALL
    
    h.   -mkdir .ssh
    
    i.   -chmod 700 .ssh
    
    j.   -touch .ssh/authorized_keys
    
    
    h.  once user grader created, login to grader via -
            ssh -i linuxProject grader@52.43.165.216
    
    i.  Unless otherwise specified all future actions taken as user 'grader'
    
3.  Establish Firewalls (most of this is redundant as they were set in Lightsail)

    a.  -sudo ufw default deny incoming
    
    b.  -sudo ufw default allow outgoing
    
    c.  -sudo ufw allow ssh
    
    d.  -sudo ufw allow 2200/tcp
    
    e.  -sudo ufw allow 22/tcp
    
    f.  -sudo ufw allow www
    
    g.  -sudo ufw enable
    
4.  Deploy Flask Application--relied heavily on this document (which was referenced throughout the forums):  https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps

    a.  install and enable mod_wsgi
            -sudo apt-get install libapache2-mod-wsgi python-dev
            
            -sudo a2enmod wsgi 
            
    b.  create flask app
            move to the /var/www directory via - cd /var/www
            
            - sudo mkdir FlaskApp
            
            move into newly created directory via -cd FlaskApp
            
            - sudo mkdir FlaskApp
            
            move into newly created directory via -cd FlaskApp
            
            make two directories, static and templates via -cd mkdir static templates
            
            -sudo nano __init__.py then add the following file:
            
                from flask import Flask
                app = Flask(__name__)
                @app.route("/")
                def hello():
                    return "Hello, I love Digital Ocean!!!!"
                if __name__ == "__main__":
                    app.run()
        Save file and close
        
    c.  Install Flask
        
        -sudo pip install virtualenv
        -sudo virtualenv venv
        -source venv/bin/activate
        -sudo pip install Flask
        -sudo python __init__.py
        
    d.  Configure and enable new virtual host
    
        -sudo nano /etc/apache2/sites-available/FlaskApp.conf
        
        Add in the following code:
            
            <VirtualHost *:80>
                ServerName 52.43.165.216
                ServerAdmin admin@mywebsite.com
                WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
                <Directory /var/www/FlaskApp/FlaskApp/>
                    Order allow,deny
                    Allow from all
                </Directory>
                Alias /static /var/www/FlaskApp/FlaskApp/static
                <Directory /var/www/FlaskApp/FlaskApp/static/>
                    Order allow,deny
                    Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
            </VirtualHost>
    save file and close
    
        -sudo a2ensite FlaskApp
        
    e.  Create the .wsgi file
    
        


