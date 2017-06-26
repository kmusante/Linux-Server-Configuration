# Linux-Server-Configuration
Linux Server Configuration Project for Udacity's FSND

This project is meant to satisfy the project Rubric defined here:  https://review.udacity.com/#!/rubrics/7/view

pw for user grader='grader'

passphrase for key 'linuxProject' = 'austerity'

pw for user catalog='catalog'

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

2.  Updated Packages and added user 'grader'
    a. sudo apt-get update
    
    b.  sudo apt-get upgrade
    
    c.  sudo apt-get autoremove
    
    d.  sudo apt-get install finger
    
    e.  sudo adduser grader
            set pw
    f.  sudo visudo and added grader to ALL=(ALL: ALL) ALL
    
    g.  once user grader created, login to grader via ssh -i linuxProject grader@52.43.165.216
    
3.  Establish Firewalls (most of this is redundant as they were set in Lightsail)

    a.  sudo ufw default deny incoming
    
    b.  sudo ufw default allow outgoing
    
    c.  sudo ufw allow ssh
    
    d.  sudo ufw allow 2200/tcp
    
    e.  sudo ufw allow 22/tcp
    
    f.  sudo ufw allow www
    
    g.  sudo ufw enable
    
    


