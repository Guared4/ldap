Vagrant.configure("2") do |config|
    config.vm.box = "bento/centos-stream-9"
    config.vm.provider :virtualbox do |v|
      v.memory = 2048
      v.cpus = 2
    end

    ssh_pub_key = File.readlines("./id_rsa/id_rsa.pub").first.strip
    config.vm.provision "shell", inline: <<-SHELL
      echo #{ssh_pub_key} >> ~vagrant/.ssh/authorized_keys
      echo #{ssh_pub_key} >> ~root/.ssh/authorized_keys
      sudo sed -i 's/\#PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
      systemctl restart sshd
    SHELL
  
    boxes = [
      { :name => "ipa.otus.lan",
        :ip => "192.168.57.10",
      },
      { :name => "client1.otus.lan",
        :ip => "192.168.57.11",
      },
      { :name => "client2.otus.lan",
        :ip => "192.168.57.12",
      }
    ]
    
    boxes.each do |opts|
      config.vm.define opts[:name] do |config|
        config.vm.hostname = opts[:name]
        config.vm.network "private_network", ip: opts[:ip]
      end
    end
  end
  