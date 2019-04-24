$script = <<-SCRIPT
    yum install epel-release -y
    yum install nginx vim mc nano -y
    sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/g' /etc/ssh/sshd_config
    sudo service sshd restart
    setenforce 0
    mv ~vagrant/lits.conf /etc/nginx/conf.d/
    systemctl enable nginx
    systemctl start nginx

SCRIPT
   

Vagrant.configure("2") do |config|
    config.vagrant.plugins = "vagrant-disksize"
    config.vagrant.plugins = "vagrant-vbguest"

    config.vm.define "SRV1" do |srv1|
        srv1.vm.box = "centos/7"
        srv1.vm.network "private_network", type: "dhcp"
        srv1.vm.hostname = "server1"
          srv1.vm.provider "virtualbox" do |vb|
            vb.memory = 1024
            vb.cpus = 2
          end
    end

    config.vm.define "SRV2" do |srv2|
        srv2.vm.box = "centos/7"
        srv2.vm.network "private_network", ip: "192.168.33.100", :netmask => "255.255.255.0", auto: true
        srv2.vm.hostname = "server2"
        srv2.vm.provision "file", source: "configs/lits.conf", destination: "/home/vagrant/lits.conf"
        srv2.vm.provision "shell", inline: $script
        srv2.disksize.size = '50GB'
        srv2.vm.synced_folder "www", "/var/www"
        srv2.vm.network "forwarded_port", guest: 80, host: 8080
        srv2.vm.provider "virtualbox" do |vb|
        end
        # config.vm.provider "virtualbox" do |vb|
        #   vb.gui = true
        # end
    end
end
