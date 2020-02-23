
Vagrant.configure('2') do |config|
  config.vm.box = 'centos/7'

  config
  {
    'swarm-worker'   => '192.168.33.11',
    'swarm-manager'    => '192.168.33.10',
  }.each do |short_name, ip|

    config.vm.define short_name do |host|
      host.vm.network 'private_network', ip: ip
      host.vm.hostname = "#{short_name}"

      host.ssh.insert_key = false
      vagrant_home_path = ENV["VAGRANT_HOME"] ||= "~/.vagrant.d"
      host.ssh.private_key_path = ["#{vagrant_home_path}/insecure_private_key"]
      if  ENV['SSH_KEY']
        private_key_path=File.expand_path(ENV['SSH_KEY'])
        host.ssh.private_key_path.push(private_key_path)
        public_key_path=ENV['SSH_KEY']+".pub"
        ssh_public_key=File.readlines(public_key_path).first.strip
        host.vm.provision 'shell', inline: "echo #{ssh_public_key} >> /home/vagrant/.ssh/authorized_keys", privileged: false
        host.vm.provision 'shell', inline: "mkdir /root/.ssh/ && echo #{ssh_public_key} >> /root/.ssh/authorized_keys"
        end
      end
    end
end
