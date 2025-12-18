```yaml
---
- name: Setup Development Environment
  hosts: localhost
  become: yes
  become_user: davidt
  tasks:

    # Install Flatpak applications
    - name: Install Flatpak applications
      flatpak:
        name: "{{ item }}"
        state: present
      loop:
        - com.brave.Browser
        - org.gnome.Snapshot
        - org.libreoffice.LibreOffice
        - org.remmina.Remmina
        - com.termius.Termius
        - com.slack.Slack
        - org.keepassxc.KeePassXC
        - org.mozilla.Thunderbird
        - us.zoom.Zoom
        - org.wireshark.Wireshark
        - com.google.Chrome
        - io.github.shiftey.Desktop
        - com.jgraph.drawio.desktop
        - org.adishatz.Screenshot
        - com.github.finefindus.eyedropper
        - com.obsproject.Studio
        - com.vivaldi.Vivaldi
        - com.vscodium.codium
        - org.kde.kdenlive
        - io.github.input_leap.input-leap
        - com.spotify.Client
        - org.kde.kdenlive
        - com.github.johnfactotum.Foliate
        - be.alexandervanhee.gradia

    - name: Install homebrew applications
      homebrew:
        name: "{{ item }}"
        state: present
      loop:
        - hugo

    - name: Install pip modules
      ansible.builtin.pip:
        name: "{{ item }}"
        state: latest
      loop:
        - ansible-navigator
```