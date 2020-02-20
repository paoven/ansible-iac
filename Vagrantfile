#require_relative './vagrant/key_authorization'

Vagrant.configure('2') do |config|
  config.vm.box = 'centos/7'
  #authorize_key_for_root config, '~/.ssh/dev_ed25519.pub'

  config
  {
    'swarm-worker'   => '192.168.33.11',
    'swarm-master'    => '192.168.33.10',
  }.each do |short_name, ip|
    config.vm.define short_name do |host|
      host.vm.network 'private_network', ip: ip
      host.vm.hostname = "#{short_name}"
    end
  end

        
  config.vm.provision "ansible" do |ansible|
    ansible.limit = 'dev_docker_nodes'
    ansible.inventory_path = 'inventory/hosts.ini'
    ansible.playbook = 'site.yml'
  end


end
