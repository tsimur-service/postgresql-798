$script = <<-SCRIPT
  echo '10.20.30.41 ps1-server' | sudo tee -a /etc/hosts
  echo '10.20.30.42 ps2-server' | sudo tee -a /etc/hosts
  
  sudo apt-get -y update
  apt-get -y install apt-transport-https
  echo "deb https://dl.2ndquadrant.com/default/release/apt stretch-$2ndquadrant main" > /etc/apt/sources.list.d/2ndquadrant.list
  wget --quiet -O - https://dl.2ndquadrant.com/gpg-key.asc | apt-key add -
  apt-get -y install postgresql postgresql-12-repmgr
  systemctl stop postgresql
  echo "postgres ALL = NOPASSWD: /usr/bin/pg_ctlcluster" > /etc/sudoers.d/postgres
  cp /vagrant/repmgrd /etc/default/
  chown postgres:postgres /etc/default/repmgrd

SCRIPT


$ps1 = <<-SCRIPT
  echo ps1
  cp -r /vagrant/ps1/ssh/ /var/lib/postgresql/
  chown -R postgres:postgres /var/lib/postgresql/.ssh

  cp /vagrant/1repmgr.conf /etc/repmgr.conf
  chown postgres:postgres /etc/repmgr.conf

  cp /vagrant/postgresql.conf /etc/postgresql/12/main/postgresql.conf
  chown postgres:postgres /etc/postgresql/12/main/postgresql.conf

  cp /vagrant/pg_hba.conf /etc/postgresql/12/main/pg_hba.conf
  chown postgres:postgres /etc/postgresql/12/main/pg_hba.conf

  # sudo service postgresql restart 
  # sudo service repmgrd restart 
  # ps aux | grep repmgrd
  # sudo -i -u postgres
  # createuser --replication --createdb --createrole --superuser repmgr
  # createdb repmgr -O repmgr
  # repmgr primary register
  # repmgr cluster show
SCRIPT


$ps2 = <<-SCRIPT
  echo ps2
  cp -r /vagrant/ps2/ssh/ /var/lib/postgresql/
  chown -R postgres:postgres /var/lib/postgresql/.ssh

  cp /vagrant/postgresql.conf /etc/postgresql/12/main/postgresql.conf
  chown postgres:postgres /etc/postgresql/12/main/postgresql.conf

  cp /vagrant/2repmgr.conf /etc/repmgr.conf
  chown postgres:postgres /etc/repmgr.conf

  cp /vagrant/pg_hba.conf /etc/postgresql/12/main/pg_hba.conf
  chown postgres:postgres /etc/postgresql/12/main/pg_hba.conf

  # sudo service postgresql restart 
  # sudo service repmgrd restart
  # sudo -i -u postgres
  # createuser --replication --createdb --createrole --superuser repmgr
  # createdb repmgr -O repmgr
  # exit
  # sudo systemctl stop postgresql
  # sudo -i -u postgres
  # rm -r /var/lib/postgresql/12/main/*
  # repmgr -h ps1-server -U repmgr -d repmgr standby clone --dry-run
  # repmgr -h ps1-server -U repmgr -d repmgr standby clone
  # exit
  # sudo systemctl start postgresql
  # repmgr cluster show
  # psql 'host=ps1-server dbname=repmgr user=repmgr'
SCRIPT


Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.provision "shell", inline: $script

  config.vm.define "ps1" do |ps1|
    ps1.vm.hostname = "ps1"
    ps1.vm.network "private_network", ip: "10.20.30.41",
    virtualbox__intnet: true
    ps1.vm.provision "shell", inline: $ps1
  end

  config.vm.define "ps2" do |ps2|
    ps2.vm.hostname = "ps2"
    ps2.vm.network "private_network", ip: "10.20.30.42",
    virtualbox__intnet: true
    ps2.vm.provision "shell", inline: $ps2
  end

end