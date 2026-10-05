# Installation on Linux with Ansible

## Headless installation (fpclazup)

File lazarus_vars.yml containing the user under which Lazarus should be installed: 
    
    
    ---
    lazarus_user: "lazar"

The actual playbook: 
    
    
    ---
    - hosts: 192.168.1.43
      vars_files:
              - lazarus_vars.yml
    
      tasks:
    
    # prerequisites
      - name: update packages
        become: yes
        apt: "update_cache=yes"
    
      - name: install dependencies
        become: yes
        apt: name={{item}} state=latest
        with_items:
              - "make"
              - "binutils"
              - "build-essential"
              - "gdb"
              - "subversion"
              - "zip"
              - "unzip"
              - "libx11-dev"
              - "libgtk2.0-dev"
              - "libgdk-pixbuf2.0-dev"
              - "libcairo2-dev"
              - "libpango1.0-dev"
              - "unrar"
              - "libxtst-dev"
              - "libgl1-mesa-dev"
              - "libatk-adaptor"
    
      - name: create user for Lazarus installation
        become: yes
        user: name="{{lazarus_user}}"
    
      - name: get url for latest fpclazup
        local_action:
           module: uri
           url: https://api.github.com/repos/LongDirtyAnimAlf/Reiniero-fpcup/releases/latest
           return_content: yes
           method: GET
        register: json_info
    
      - name: download latest fpclazup
        become: yes
        become-user: "{{lazarus_user}}"
        get_url:
          url: "{{json_info.json.assets[10].browser_download_url}}"
          dest: "/home/{{lazarus_user}}/"
    
      - name: make downloaded executable
        become: yes
        become-user: "{{lazarus_user}}"
        shell: chmod +x /home/{{lazarus_user}}/fpclazup-x86_64-linux
    
      - name: install 64bit
        become: yes
        become-user: "{{lazarus_user}}"
        shell: /home/{{lazarus_user}}/fpclazup-x86_64-linux --fpcURL="stable" --lazURL="stable" --installdir=usr/share/fpclazstable --fpcsplit --noconfirm

  


## Installer with GUI (Fpcupdeluxe)

File lazarus_vars.yml containing the user under which Lazarus should be installed: 
    
    
    ---
    lazarus_user: "lazar"

  
The actual playbook: 
    
    
    ---
    - hosts: 192.168.1.43
      vars_files:
              - lazarus_vars.yml
    
      tasks:
    
    # prerequisites
      - name: update packages
        become: yes
        apt: "update_cache=yes"
    
      - name: install dependencies
        become: yes
        apt: name={{item}} state=latest
        with_items:
              - "make"
              - "binutils"
              - "build-essential"
              - "gdb"
              - "subversion"
              - "zip"
              - "unzip"
              - "libx11-dev"
              - "libgtk2.0-dev"
              - "libgdk-pixbuf2.0-dev"
              - "libcairo2-dev"
              - "libpango1.0-dev"
              - "unrar"
              - "libxtst-dev"
              - "libgl1-mesa-dev"
              - "libatk-adaptor"
    
      - name: create user for Lazarus installation
        become: yes
        user: name="{{lazarus_user}}"
    
      - name: get url for latest FPCUPDeluxe
        local_action:
           module: uri
           url: https://api.github.com/repos/newpascal/fpcupdeluxe/releases/latest
           return_content: yes
           method: GET
        register: json_info
    
      - name: download latest FPCUPDeluxe
        become: yes
        become-user: "{{lazarus_user}}"
        get_url:
          url: "{{json_info.json.assets[16].browser_download_url}}"
          dest: "/home/{{lazarus_user}}/"
    
      - name: make downloaded executable
        become: yes
        become-user: "{{lazarus_user}}"
        shell: chmod +x /home/{{lazarus_user}}/fpcupdeluxe-x86_64-linux

---

_Source: [https://wiki.freepascal.org/Installation_on_Linux_with_Ansible](https://web.archive.org/web/20200928162642/https://wiki.freepascal.org/Installation_on_Linux_with_Ansible)_
