# Setup Cross Compile For ARM

│ **[English (en)](<../en/Setup_Cross_Compile_For_ARM.md> "Setup Cross Compile For ARM")** │  **[español (es)](</Setup_Cross_Compile_For_ARM/es> "Setup Cross Compile For ARM/es")** │  **[magyar (hu)](</Setup_Cross_Compile_For_ARM/hu> "Setup Cross Compile For ARM/hu")** │  **[Bahasa Indonesia (id)](</Setup_Cross_Compile_For_ARM/id> "Setup Cross Compile For ARM/id")** │  **русский (ru)** │    
****

# Ubuntu 12.04.4 LTS

Установка производилась практически на чистую систему 
    
    
    sudo apt-get install libgtk2.0-dev binutils-arm-linux-gnueabi
    mkdir /tmp/fpsinstall && cd /tmp/fpsinstall
    mkdir -p ~/dev/fpc/binutils/
    wget http://sourceforge.net/projects/lazarus/files/Lazarus%20Linux%20amd64%20DEB/Lazarus%201.2/lazarus_1.2.0-0_amd64.deb/download -O lazarus_1.2.0-0_amd64.deb
    wget http://sourceforge.net/projects/lazarus/files/Lazarus%20Linux%20amd64%20DEB/Lazarus%201.2/fpc-src_2.6.2-0_amd64.deb/download -O fpc-src_2.6.2-0_amd64.deb
    wget http://sourceforge.net/projects/lazarus/files/Lazarus%20Linux%20amd64%20DEB/Lazarus%201.2/fpc_2.6.2-0_amd64.deb/download -O fpc_2.6.2-0_amd64.deb
    sudo dpkg -i *.deb
    rm -rf /tmp/fpsinstall
    cd /usr/share/fpcsrc/2.6.2
    ln -s /usr/bin/arm-linux-gnueabi-ar /home/$USER/dev/fpc/binutils/ar
    ln -s  /usr/bin/arm-linux-gnueabi-as /home/$USER/dev/fpc/binutils/as
    ln -s /usr/bin/arm-linux-gnueabi-ld /home/$USER/dev/fpc/binutils/ld
    sudo make crossinstall CPU_TARGET=arm OS_TARGET=linux CROSSBINDIR=~/dev/fpc/binutils/ OPT=-dFPC_ARMEL INSTALL_PREFIX=/usr
    sudo ln -sf /usr/lib/fpc/2.6.2/ppcrossarm /usr/bin/ppcarm
    sudo ln -s  /usr/bin/arm-linux-gnueabi-as /usr/bin/arm-linux-as
    sudo ln -s  /usr/bin/arm-linux-gnueabi-ld /usr/bin/arm-linux-ld
    

Далее, как в примере англоязычной версии этой статьи 
    
    
    fpc -Tlinux -Parm -XParm-linux- TestARM.pas
    

Скомпилированный файл успешно запустился на Droid4.

---

_Source: [https://wiki.freepascal.org/Setup_Cross_Compile_For_ARM/ru](https://web.archive.org/web/20240807122429/https://wiki.freepascal.org/Setup_Cross_Compile_For_ARM/ru)_
