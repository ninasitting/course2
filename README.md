# Configuring a Jupyter Data Science Notebook Server

## SSH Keys 
### In terminal/bash on local computer:
    ls ~/.ssh                      #to see if we already have SSH keys made
    mkdir -p .ssh                  #makes the directory for our SSH keys ( -p will prevent it from making it if it already                          
                                        exists)
    ssh-keygen                       #remember we don't want a passprase
    cat ~/.ssh/id_rsa.pub | pbcopy   #copying the public key of our SSH key to our clipboard so we can add it to AWS Key Pairs
    


