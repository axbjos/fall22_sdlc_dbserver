# -*- mode: ruby -*-
# vi: set ft=ruby :

# Single Server LAMP Stack Running the PHP Crude CRUD Web Application
# See the GitHub repo for details

# Vagrant uses Ruby, so Ruby Do Loop...
Vagrant.configure("2") do |config|
  
    # Use an appropriate box here!!!!! Uncomment the line applicable to you!!!!
    # For Intel Windows/Mac using Virtualbox (and only Virtualbox) use
    # In other words for an Intel PC, uncomment the line below and comment (or remove)
    # the lines for an ARM based Mac...
    config.vm.box = "ubuntu/focal64"
  
    # Set this up so it always get the same host only address
    # or could do a port forward
    #####for host only make sure you know what you are doing - ask for help!!!
    config.vm.network "private_network", ip: "192.168.56.10"
  
    # Database server provisioning next
  
    # Stage the configurations files needed - make sure they are in your local directory for 
    # copying out to the VM
    # addusers.sql is a script file used to add the needed users to the MySQL database
    config.vm.provision "file", source: "addusers.sql", destination: "addusers.sql"
    config.vm.provision "file", source: "50-server.cnf", destination: "50-server.cnf"
  
    # Now configure the database
    # These commands will install the MariaDB distribution of MySQL
    config.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt install -y mariadb-server
      git clone https://github.com/datacharmer/test_db.git
      cd  test_db
      mysql -t < employees.sql
      cd ..
      # uncomment the next two if you want the DB accessible on all interfaces
      cp 50-server.cnf /etc/mysql/mariadb.conf.d/
      systemctl restart mariadb
      cd /home/vagrant
      echo "securing mysql and adding joeaxberg user"
      mysql -t < addusers.sql
      echo "done"
    SHELL
  
  end