# How to setup Jenkins on a Virtual Machine:


Vagrant init
Write this code into vagrant file:

```ruby
Vagrant.configure("2") do |config| 

   # Operating system
  config.vm.box = "ubuntu/bionic64"
  # IP address
  config.vm.network "private_network", ip: "192.168.10.100"
  # Alias as an address
  config.hostsupdater.aliases = ["jenkinsbox.local"]
  # Link to provision file
  config.vm.provision "shell", path: "provision.sh"

end
```

touch provision.sh
Write this into provision file:

```
sudo apt-get update
sudo apt-get upgrade

# install java in the right location
sudo apt-get install openjdk-8-jdk openjdk-8-jre -y
cd /etc

# change permission to allow the installation
sudo chmod 777 environment
cat >> /etc/environment <<EOL
JAVA_HOME= /usr/lib/jvm/java-8-openjdk-amd64
JRE_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre
EOL

# change permission back to what it was
sudo chmod 644 environment

# with java we can now install Jenkins
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'

# make sure you have the latest version
sudo apt-get update
sudo apt-get install jenkins -y

```
 
