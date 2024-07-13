Vagrant.configure("2") do |config|
  config.vm.define "dokaclass" do |dockaclass|
    dockaclass.vm.box = "generic/rocky9"
    dockaclass.vm.box_check_update = false
    dockaclass.vm.hostname = "dokaclass"
    dockaclass.vm.network "private_network", ip: "192.168.10.2"

    dockaclass.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = 2
      vb.name = "dokaclass"
    end

    dockaclass.vm.provision "shell", inline: <<-SHELL
      # Update system and install Docker
      sudo dnf check-update
      sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
      sudo dnf install -y docker-ce docker-ce-cli containerd.io

      # Start and enable Docker service
      sudo systemctl start docker
      sudo systemctl enable docker

      # Add user 'student' and add to 'docker' group
      sudo useradd student
      echo "student:password" | sudo chpasswd
      sudo usermod -aG docker student
      sudo usermod -aG wheel student

      # Allow 'student' to use sudo without a password (optional)
      echo "student ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/student

      # Update /etc/hosts to include the hostname
      echo "192.168.10.2 dockerclass.example.com dokaclass" | sudo tee -a /etc/hosts
    SHELL
  end
end
