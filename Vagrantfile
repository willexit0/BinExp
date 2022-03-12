# vagrant plugin install vagrant-proxyconf
proxy = "http://192.168.0.103:7890"

Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/bionic64"

    config.proxy.http     = "#{proxy}"
    config.proxy.https    = "#{proxy}"
    config.proxy.no_proxy = "localhost,127.0.0.1"
    
    config.vm.provision "shell", inline: <<-SHELL
    export DEBIAN_FRONTEND=noninteractive 
    dpkg --add-architecture i386
    cp /etc/apt/sources.list /etc/apt/sources.list.old

    apt-get update
    apt-get install -y zsh curl wget libc6:i386 libncurses5:i386 libstdc++6:i386 gdb python3-pip libssl-dev gcc git binutils socat apt-transport-https ca-certificates libc6-dev-i386 python-capstone libffi-dev
    pip3 install capstone unicorn keystone-engine ropper

    # wget -q -O- https://github.com/hugsy/gef/raw/master/scripts/gef.sh | sh
    wget -O /tmp/gef.sh https://github.com/hugsy/gef/raw/master/scripts/gef.sh && bash /tmp/gef.sh
    wget -O /tmp/ohmyzsh.sh https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh && bash /tmp/ohmyzsh.sh
    # wget -P $ZSH_CUSTOM/themes https://raw.githubusercontent.com/sebastianpulido/oh-my-zsh/main/smoothmonkey.zsh-theme
    sed -i "/ZSH_THEME=\"/c\ZSH_THEME=\"steeef\"" ~/.zshrc

    git clone https://github.com/niklasb/libc-database.git /home/ubuntu/libc-database
    cd /home/ubuntu/libc-database
    /home/ubuntu/libc-database/add /lib/i386-linux-gnu/libc.so.6
    /home/ubuntu/libc-database/add /lib/x86_64-linux-gnu/libc.so.6
    
    apt-get update
    SHELL
end 
