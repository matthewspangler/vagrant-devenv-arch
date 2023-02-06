Vagrant.configure("2") do |config|

  config.vm.box = "archlinux/archlinux"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "8192"
    vb.cpus = 4
    # >9MB vram necessary for seamless mode:-
    vb.customize ["modifyvm", :id, "--vram", "256"]
    # Enable 3D acceleration:
    vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
    vb.gui = true
    vb.name = "Arch Dev Box"
  end

  # Directory for files related to configuration of the VM:
  config.vm.synced_folder  "custom/", "/home/vagrant/custom", disabled: false

  # Directory for sharing my code between host and guest:
  config.vm.synced_folder "code/", "/home/vagrant/code", disabled: false

  # Share Emacs config between host and guest:
  config.vm.synced_folder "emacs/", "/home/vagrant/.emacs.d", disabled: false

  # Share dotfiles between host and guest:
  config.vm.synced_folder "dotfiles/", "/home/vagrant/dotfiles", disabled: false

  # Update + upgrade system
  config.vm.provision :shell, inline: "pacman -Syyu --noconfirm"

  # Script to install pacman packages detailed in ./custom/packages.txt, runs every time VM boots:
  config.vm.provision :shell, inline: "pacman -Sy --noconfirm --needed - < /home/vagrant/custom/pkglist.txt"
  # pkglist made with: 'pacman -Qqen > pkglist.txt'

  # Replace nox guest utils with guest utils that support x11:
  #config.vm.provision :shell, inline: "yes | pacman -S --noconfirm virtualbox-guest-qutils"

  # Enable SDDM so we can launch KDE Plasma:
  config.vm.provision :shell, inline: "systemctl enable sddm"

  # Script to setup my dotfiles using my github repo:
  config.vm.provision :shell, path: "custom/setup_dotfiles.sh", privileged: false

end
