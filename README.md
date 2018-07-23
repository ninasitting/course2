# Configuring a Jupyter Data Science Notebook Server

## SSH Keys 
### In terminal/bash on local computer:
    ls ~/.ssh                      #to see if we already have SSH keys made
    mkdir -p .ssh                  #makes the directory for our SSH keys ( -p will prevent it from making it if it already                          
                                        exists)
    ssh-keygen                       #remember we don't want a passprase
    cat ~/.ssh/id_rsa.pub | pbcopy   #copying the public key of our SSH key to our clipboard so we can add it to AWS Key Pairs
    
## AWS Setup
### On the AWS website in the EC2 section
#### Go to Key Pairs -> Import Key
   Paste in the SSH key we just copied from bash 
   ** Make sure to give it a name that will help you remember where it is from
        (it's useful to include the device and the month and year of when you created it)
        
#### Go to Security Groups -> Create New Security Group
 We are only going to be adding to the Inbound Rules and should include the following 
 
        - SSH
        - HTTP
        - HTTPS
        - postgreSQL
        - CustomTCP with a port range of 27016 
        
     ** all of these should have "Anywhere" under source **
     
#### Launching our EC2 Instance 
##### Back in the EC2 Dashboard -> Launch Instance 
    ** We will not be using tabs 3 and 5 in setting up our instance **
    1) Choose AMI : Ubuntu Server 16.04 LTS 
    2) Choose Instance Type: t2.micro
    4) Add Storage: set size to 20
    6) Configure Security Group: Select Existing -> Use the group we just made!
    
    Hit "Launch" and choose the existing Key Pair that we just imported
    
    

