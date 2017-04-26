Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network :forwarded_port, guest: 4567, host: 4567
  config.ssh.username = "vagrant"
  config.ssh.password = "vagrant"

  config.vm.provision "bootstrap",
    type: "shell",
    inline: <<-SHELL
      sudo apt-get install python-software-properties
      sudo apt-add-repository ppa:brightbox/ruby-ng
      sudo apt-get update
      sudo apt-get install -yq ruby2.2 ruby2.2-dev pkg-config build-essential nodejs git libxml2-dev libxslt-dev
      sudo ln -nsf /usr/bin/ruby2.2 /usr/bin/ruby2.0
      sudo ln -nsf /usr/bin/gem2.2 /usr/bin/gem2.0
      sudo apt-get autoremove -yq
      gem2.0 install --no-ri --no-rdoc bundler
    SHELL

  # add the local user git config to the vm
  config.vm.provision "file", source: "~/.gitconfig", destination: ".gitconfig"

  config.vm.provision "install",
    type: "shell",
    privileged: false,
    inline: <<-SHELL
      echo "=============================================="
      echo "Installing app dependencies"
      cd /vagrant
      bundle config build.nokogiri --use-system-libraries
      bundle install
    SHELL

  config.vm.provision "run",
    type: "shell",
    privileged: false,
    run: "always",
    inline: <<-SHELL
      echo "=============================================="
      echo "Starting up middleman at http://localhost:4567"
      echo "If it does not come up, check the ~/middleman.log file for any error messages"
      cd /vagrant
      bundle exec middleman server --force-polling --latency=1 &> ~/middleman.log &
    SHELL
end
