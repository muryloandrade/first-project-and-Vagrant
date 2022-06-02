$script_mysql = <<-SCRIPT
  apt-get update && \
  apt-get install -y mysql-server-5.7 && \
  mysql -e "create user 'phpuser'@'%' identified by 'pass';"
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  
  config.vm.network "forwarded_port", guest:80, host:8098
  config.vm.network "public_network", ip:"172.20.0.57", bridge: "wlp0s20f3"
  
  config.vm.provision "shell",
  inline:"apt-get install -y nginx"
  config.vm.provision "shell", 
  inline:"cat /configs/id.public >> .ssh/authorized_keys"
  config.vm.provision "shell", 
  inline: $script_mysql
  config.vm.provision "shell", 
  inline:"cat /configs/mysqld.cnf > /etc/mysql/mysql.conf.d/mysqld.cnf"
  config.vm.provision "shell", 
  inline:"service mysql restart"
  
  config.vm.synced_folder "./configs", "/configs"
  config.vm.synced_folder ".", "/vagrant", disabled: true
end
