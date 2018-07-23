# Configuring a Jupyter Data Science Notebook Server

## SSH Keys 
### In terminal/bash on local computer:
    ls ~/.ssh                      # to see if we already have SSH keys made
    mkdir -p .ssh                  # makes the directory for our SSH keys ( -p will prevent it from making it if it already                          
                                        exists)
    ssh-keygen                       # remember we don't want a passprase
    cat ~/.ssh/id_rsa.pub | pbcopy   # copying the public key of our SSH key to our clipboard so we can add it to AWS Key Pairs
    
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
    
    Once it is launched, copy the public IP address in the Description section 
    
### Docker Installation 
#### Back in terminal/bash on our local computer
    ssh ubuntu@<ip>                          # putting in the public IP address we copied from the instance we are running
                                             # when prompted type 'yes'
                                             # if it says permission denied try -i ~/.ssh/id_rsa at the end of it 
    curl -sSL https://get.docker.com | sh    # MAKE SURE YOU TYPE THIS CORRECTLY, we are telling it to download the script 
                                                and run it immediately
                                                
    sudo usermod -aG docker ubuntu           # adding the ubuntu user to to the docker group
    
    ** type ctrl-D to disconnect so this security change can be applied when we reconnect to our AWS server **
    
    ssh ubuntu@<ip>                          # reconnecting 
    tmux                                     # makes it more stable
    
### Obtaining and Running the Jupyter Notebook Image as a Container on our Server
#### Still in the same terminal/bash session    
   
    docker pull jupyter/datascience-notebook
    
    docker run \                        # make sure there are no spaces after any of the backslashes
    -d \                                # detached mode
    -p 443:8888 \                       # telling which ports to use
    -v /home/ubuntu:/home/jovyan \      # mounting the volume with a two-way connection
    jupyter/datascience-notebook        # telling it to run the notebook
    
    docker ps                           # tells us what processes are running on docker and allows us to get the container 
                                            id to use to obtain the tolken 
    docker exec [<containerid>] jupyter notebook list    # will give us the token to use so no one can access our jupyter
    
    ** Copy the token **
    
#### Now in our browser
    type "https://<publicipaddress>:443
    paste the token in when prompted
    
    YAY! You did it!! (-:
    

## Visualization of the System 

![system visualization](https://github.com/ninasitting/course2/blob/master/Screen%20Shot%202018-07-23%20at%203.46.46%20PM.png)

Note: We are connecting to AWS from our local machine through the shell using our SSH key and then using tmux pointed at the same AWS to make the connection more stable like this:

![shell connections](https://github.com/ninasitting/course2/blob/master/Screen%20Shot%202018-07-23%20at%204.08.05%20PM.png)

    
## Pricing
    ** for 3 months **
    
    
    t2.micro = $25.40
    t2.small = $50.37
    t2.medium = $101.62
    
    
