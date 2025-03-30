#:[.'.]:>--------------------------------------------------
#:[.'.]:> Marco Antonio - markitos
#:[.'.]:> Markitos DevSecOps Kulture - El camino del artesan@
#:[.'.]:> markitos.es.info@gmail.com
#:[.'.]:> https://github.com/markitos-es
#:[.'.]:> https://discord.com/invite/YZWm2cYF 
#:[.'.]:>--------------------------------------------------

# First Steps

## As Normal User (SET MY_* VARS)
Switch to the root user:
```sh
export MY_PASSWORD=alatrist3DSG18j.
export MY_USERNAME=markitos
export MY_HOSTNAME=teseo
env | grep -E 'MY_USERNAME|MY_PASSWORD|MY_HOSTNAME'

# AMBOS

sudo -i su
export MY_PASSWORD=alatrist3DSG18j.
export MY_USERNAME=markitos
export MY_HOSTNAME=teseo
env | grep -E 'MY_USERNAME|MY_PASSWORD|MY_HOSTNAME'
```



## As Root
Configure sudoers, update and install necessary packages, set hostname, and configure hosts file:
```sh
echo "${MY_USERNAME}    ALL = (ALL) NOPASSWD: ALL" >>/etc/sudoers &&
    apt update && apt upgrade -y && apt install -y \
    ccze software-properties-common nano git sshpass make progress docker-compose-v2 docker.io btop \
    curl apt-transport-https ca-certificates curl net-tools && \
    updatedb
hostnamectl set-hostname "${MY_HOSTNAME}"
echo "" >> /etc/hosts
echo "#:{.'.}>----- ${MY_HOSTNAME} network -----" >> /etc/hosts
echo "192.168.1.200 prometeo1" >> /etc/hosts
echo "192.168.1.250 titan1"   >> /etc/hosts
echo "192.168.1.251 titan2"   >> /etc/hosts
echo "192.168.1.252 titan3"   >> /etc/hosts
echo "#:{.'.}>----- ${MY_HOSTNAME} network -----" >> /etc/hosts
echo "" >> /etc/hosts

echo "" >> /etc/bash.bashrc
echo "#:{.'.}>" >> /etc/bash.bashrc
echo "#:{.'.}>----- startof.aliases ${MY_HOSTNAME} -----" >> /etc/bash.bashrc
echo "alias prometeo1=\"sshpass -p ${MY_PASSWORD} ssh ${MY_USERNAME}@prometeo1\"" >> /etc/bash.bashrc
echo "alias titan1=\"sshpass -p ${MY_PASSWORD} ssh ${MY_USERNAME}@titan1\"" >> /etc/bash.bashrc
echo "alias titan2=\"sshpass -p ${MY_PASSWORD} ssh ${MY_USERNAME}@titan2\"" >> /etc/bash.bashrc
echo "alias titan3=\"sshpass -p ${MY_PASSWORD} ssh ${MY_USERNAME}@titan3\"" >> /etc/bash.bashrc
echo "#:{.'.}>----- endof.alias ${MY_HOSTNAME} -----" >> /etc/bash.bashrc
echo "#:{.'.}>" >> /etc/bash.bashrc
echo "#:{.'.}>----- startof.misc-sets ${MY_HOSTNAME} -----" >> /etc/bash.bashrc
echo "export LANG=es_ES.UTF-8" >> /etc/bash.bashrc
echo "if [[ -n \$SSH_CONNECTION ]]; then" >> /etc/bash.bashrc
echo "   export EDITOR='nano'" >> /etc/bash.bashrc
echo " else" >> /etc/bash.bashrc
echo "   export EDITOR='nano'" >> /etc/bash.bashrc
echo "fi" >> /etc/bash.bashrc
echo "#:{.'.}>----- endof.misc-sets ${MY_HOSTNAME} -----" >> /etc/bash.bashrc
echo "" >> /etc/bash.bashrc
exit
```

## As Normal User
Add the current user to the Docker group and change ownership of the Docker socket:
```sh
sudo usermod -aG docker $USER && \
sudo chown $USER /var/run/docker.sock
```

# Languages Setups

## Go with Goenv
### Option 1: Goenv as Normal User
# Install Goenv and set up Go environment:
# ```sh
# git clone https://github.com/go-nv/goenv.git ~/.goenv
# echo "" >> ~/.bashrc
# echo "#:{.'.}>----- golang sets -----" >> ~/.bashrc
# echo 'export GOENV_ROOT="$HOME/.goenv"' >> ~/.bashrc
# echo 'export PATH="$GOENV_ROOT/bin:$PATH"' >> ~/.bashrc
# echo 'eval "$(goenv init -)"' >> ~/.bashrc
# echo 'export PATH="$GOROOT/bin:$PATH"' >> ~/.bashrc
# echo 'export PATH="$PATH:$GOPATH/bin"' >> ~/.bashrc
# echo "#:{.'.}>----- golang sets -----" >> ~/.bashrc
# echo "" >> ~/.bashrc
# source ~/.bashrc
# goenv install 1.24.1
# goenv global 1.24.1
# go version
# ```

### Option 2: Go as Root
# Download and install Go manually:
# ```sh
# sudo su
# cd /tmp
# wget https://go.dev/dl/go1.24.1.linux-amd64.tar.gz -O go.tar.gz
# tar xvfz go.tar.gz 
# mv go /usr/local/
# echo 'export PATH=$PATH:/usr/local/go/bin' >> /etc/bash.bashrc
# exit
# ```

## Node with NVM
# Install NVM and set up Node.js:
# ```sh
# cd /tmp
# curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
# source ~/.bashrc
# nvm install v20.17.0
# nvm use v20.17.0
# nvm version
# node -v
# npm -v
# ```

# # Other Tuning Tools
# 
# Install Oh My Bash and configure plugins and aliases (current my-theme is copied-duru really cool :D):
# ```sh
# bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
# nano ~/.bashrc
# completions=(
#   git
#   composer
#   ssh
#   nvm
#   go
# )
# 
# aliases=(
#   general
# )
# 
# plugins=(
#   git
#   bashmarks
#   golang
#   goenv
#   nvm
# )
# 
# ```
