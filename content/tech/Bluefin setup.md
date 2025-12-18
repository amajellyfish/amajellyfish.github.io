Run command
`ujust devmode`

Add yourself to the right groups
`ujust dx-group`

https://docs.projectbluefin.io/bluefin-dx/

Flatpaks
- nextcloud desktop
- Chrome
- Brave
- Slack
- Spotify
- Remmina
- Termius

```yml
---
- name: Setup Development Environment
  hosts: localhost
  become: yes
  tasks:

    # Install Flatpak applications
    - name: Install Flatpak applications
      flatpak:
        name: "{{ item }}"
        state: present
      loop:
        - com.bitwarden.desktop
        - com.brave.Browser
        - org.gimp.GIMP
        - org.gnome.Snapshot
        - org.libreoffice.LibreOffice
        - org.remmina.Remmina
        - com.termius.Termius
        - com.slack.Slack
        - org.keepassxc.KeePassXC
        - md.obsidian.Obsidian
        - com.calibre_ebook.calibre
        - org.mozilla.Thunderbird
        - us.zoom.Zoom
        - org.wireshark.Wireshark
        - com.google.Chrome
        - io.github.shiftey.Desktop
        - io.github.dvlv.boxbuddyrs
        - com.github.tchx84.Flatseal
        - io.github.flattool.Warehouse
        - io.missioncenter.MissionCenter
        - com.github.rafostar.Clapper
        - com.mattjakeman.ExtensionManager
        - com.jgraph.drawio.desktop
        - org.adishatz.Screenshot
        - com.github.finefindus.eyedropper
        - com.github.johnfactotum.Foliate
        - com.obsproject.Studio
        - com.vivaldi.Vivaldi
        - com.vscodium.codium
        - io.podman_desktop.PodmanDesktop
        - org.kde.kdenlive
        - org.virt_manager.virt-manager
        - io.github.input_leap.input-leap
        - com.nextcloud.desktopclient.nextcloud
```

Homebrew
- hugo

bashrc
```bash
export PATH=$PATH:/usr/local/go/bin
export PATH=$PATH:/home/davidt/Nextcloud/Documents/Scripts
export HISTTIMEFORMAT='%F %T '

# Set up Vagrant provider
export VAGRANT_DEFAULT_PROVIDER=libvirt

# Tell libvirt to use system as default URI
export LIBVIRT_DEFAULT_URI="qemu:///system"

alias server='ssh 10.0.10.56'
alias gitu='git add . && git commit -m "Commit" && git push'
alias sombrero='ssh m104@sombrero'
alias oort='ssh m104@10.0.10.51'
alias control='ssh ansible@control'
alias proxmox1='ssh root@172.16.1.39'
alias hachiwara='ssh root@hachiwara'
alias ansible1='ssh ansible@ansible1'
alias ansible2='ssh ansible@ansible2'
alias gitu='git add . && git commit -m "Commit" && git push'
alias ansd='cd ~/Nextcloud/Documents/projects/base'
alias labup='sudo virsh start Ansible1 && sudo virsh start Ansible2'
alias labdown='sudo virsh shutdown Ansible1 & sudo virsh shutdown Ansible2'
alias scripts='cd ~/Nextcloud/Documents/Scripts'
alias oldoort='ssh root@oldoort'
alias cosharps='sudo podman run -it --network host --rm almalinux:8 bash -c "yum install -y openssh-clients && ssh -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedKeyTypes=+ssh-rsa     -L 8081:hachiwara:80 -p 55555 cpbuser@cutlass.psi.edu"'
alias flushdns='sudo systemd-resolve --flush-caches'

```

Ansible:

`python3 -m pip install --user ansible`
For libvirt:
`rpm-ostree install --allow-inactive python3-libvirt python3-lxml`

Generate public key:
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519

Virt manager dark mode
go to gnome tweaks and set the theme for lagacy apps to adw-gtk3-dark

```
export PATH=$PATH:/home/davidt/Nextcloud/Documents/Scripts
export HISTTIMEFORMAT='%F %T '
export LIBVIRT_DEFAULT_URI="qemu:///system"

alias server='ssh 10.0.10.56'
alias gitu='git add . && git commit -m "Commit" && git push'
alias sombrero='ssh dthomas@sombrero'
alias oort='ssh m104@10.0.10.51'
alias control='ssh ansible@control'
alias proxmox1='ssh root@172.16.1.39'
alias hachiwara='ssh root@hachiwara'
alias web1='ssh ansible@web1'
alias db1='ssh ansible@db1'
alias ftp1='ssh ansible@ftp1'
alias gitu='git add . && git commit -m "Commit" && git push'
alias ansd='cd ~/Nextcloud/Documents/projects/base'
alias labup='sudo virsh start control && sudo virsh start db1 && sudo virsh start ftp1 && sudo virsh start web1'
alias labdown='sudo virsh shutdown Ansible1 & sudo virsh shutdown Ansible2'
alias scripts='cd ~/Nextcloud/Documents/Scripts'
alias cosharps='sudo podman run -it --network host --rm almalinux:8 bash -c "yum install -y openssh-clients && ssh -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedKeyTypes=+ssh-rsa     -L 8081:hachiwara:80 -p 55555 cpbuser@cutlass.psi.edu"'
alias flushdns='sudo systemd-resolve --flush-caches'

```

extensions: tiling shell
need to copy and note settings used
 
hotkeys
extentions > tiling shell 
super + direction = move windows
ctrl + direction = switch window focus


Gnome shortcuts
alt + left/righ = switch between workspaces
alt + t New terminal window
alt + c Close selected Window
