Vagrant.configure("2") do |config|
  config.vm.box = "geerlingguy/centos7"
  config.vm.network "private_network", ip: "192.168.33.16"

  config.ssh.insert_key = false
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.name = "jenkins"
    vb.memory = "1024"
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo yum update -y
    sudo yum install java-1.8.0-openjdk-devel -y

    curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo
    sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key

    sudo yum install jenkins -y
    sudo systemctl start jenkins

    sudo systemctl enable jenkins

    sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
    sudo firewall-cmd --reload

    sudo cat admin > /var/lib/jenkins/secrets/initialAdminPassword
  SHELL
end
