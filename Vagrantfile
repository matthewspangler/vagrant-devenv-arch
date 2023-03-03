Vagrant.configure("2") do |config|

  config.vm.box = "archlinux/archlinux"

  # Directory for sharing my code between host and guest:
  config.vm.synced_folder "home/",
                        "/home/vagrant/",
                        type: "rsync",
                        rsync__args: ["--verbose",
                                      "--rsync-path='sudo rsync'",
                                      "--archive",
                                      "--delete",
                                      "-z"],
                        owner:"vagrant",
                        group: "vagrant"

  config.vm.provider :libvirt do |libvirt|
    #libvirt.host = "system"
    libvirt.memory = "8192"
    libvirt.cpus = 4
    # libvirt.video_vram = ""
  end

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

  # Update + upgrade system
  config.vm.provision :shell, inline: "pacman -Syyu --noconfirm"

  # Install important packages, rest handled by dotfiles installer
  config.vm.provision :shell, inline: <<-SHELL
  pacman -Sy --noconfirm --needed \
  alsa-firmware \
  alsa-plugins \
  alsa-utils \
  ansible \
  archlinux-keyring \
  ark \
  automake \
  awesome-terminal-fonts \
  base \
  bash-completion \
  bleachbit \
  breeze-gtk \
  chromium \
  clang \
  cryptsetup \
  ctags \
  dnsmasq \
  docker \
  docker-compose \
  dolphin \
  emacs \
  gcc \
  git \
  glances \
  gnu-netcat \
  make \
  okular \
  p7zip \
  pkgconf \
  pkgfile \
  plasma-desktop \
  plasma-disks \
  plasma-nm \
  plasma-pa \
  python \
  python-capng \
  python-defusedxml \
  python-dotenv \
  python-jupyter_client  \
  python-jupyterlab_server \
  python-odfpy \
  python-packaging \
  python-pip \
  python-pyqt5 \
  python-pywal \
  rsync \
  rust-analyzer  \
  rustup \
  sddm \
  stow \
  thefuck  \
  tldr \
  tmux \
  unrar \
  unzip \
  vim \
  weechat \
  wget \
  xonsh \
  xorg-server \
  xorg-xdpyinfo \
  xorg-xinit \
  xorg-xinput \
  xorg-xkill \
  xorg-xrandr \
  xterm \
  yakuake \
  zsh
SHELL

  # Replace nox guest utils with guest utils that support x11:
  #config.vm.provision :shell, inline: "yes | pacman -S --noconfirm virtualbox-guest-qutils"

  # Enable SDDM so we can launch KDE Plasma:
  config.vm.provision :shell, inline: "systemctl enable sddm"

  # Setup my dotfiles using my github repo:
  config.vm.provision :shell, privileged: false, inline: <<-SETUP
  cd ~
  git clone https://github.com/matthewspangler/dotfiles
  cd ./dotfiles
  chmod a+x ./install.sh
  ./install.sh -p vagrant
SETUP


end
