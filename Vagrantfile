# -*- mode: ruby -*-
# vi: set ft=ruby :
# Where to store the disk file


disk = '.\swarm000_disk.vdi'



VAGRANTFILE_API_VERSION = "2"

cluster = {
   "swarm01.local.box" => { :ip => "192.168.56.111", :cpus => 2, :mem => 4096 , :boot_size=>100} ,
   "swarm02.local.box" => { :ip => "192.168.56.112", :cpus => 2, :mem => 4096 , :boot_size=>100},
   "swarm03.local.box" => { :ip => "192.168.56.113", :cpus => 2, :mem => 4096 , :boot_size=>100}
}
 #<Display controller="VMSVGA"/>
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  cluster.each_with_index do |(hostname, info), index|

    config.vm.define hostname do |cfg|
      cfg.vm.provider :virtualbox do |vb, override|
		config.vm.box = "bento/centos-7.9"
		#config.vm.box = "centos/7"


		override.vm.network :private_network, ip: "#{info[:ip]}"
        override.vm.hostname = hostname
        vb.name = hostname
        #vb.customize ["modifyvm", :id, "--memory", info[:mem], "--cpus", info[:cpus], "--hwvirtex", "on" ]
        vb.customize ["modifyvm", :id, "--memory", info[:mem], "--cpus", info[:cpus], "--hwvirtex", "on"  ,"--graphicscontroller","vmsvga"  ]
		disk =  './'+ hostname +'_swarm_u01_disk.vdi'
		

	    unless File.exist?(disk)
			vb.customize ['createhd', '--filename', disk, '--size', 1 * 2048 ]
		end
		vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', disk]


		#ssh keys
		#https://medium.com/@Sohjiro/add-public-key-to-vagrant-4bd5424521bf
		#config.ssh.insert_key = false # 1
		#config.ssh.private_key_path = ['~/.vagrant.d/insecure_private_key', '~/.ssh/id_rsa'] # 2
		#config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys" # 3
		# 4
		  # config.vm.provision "shell", inline: <<-EOC
			# sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
			# sudo systemctl restart sshd.service
			# echo "finished"
		  # EOC

		# add DVD 
#		vb.customize ["storageattach", :id, 
#                "--storagectl", "SATA Controller", 
#                "--port", "2", "--device", "20", 
#                "--type", "dvddrive", 
#                "--medium", "emptydrive"]

		#vb.customize [ modifyhd ,0 , --resize , 100000 ]
      end # end provider
	
    end # end config

  end # end cluster
end
