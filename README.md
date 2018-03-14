# Archomatic
Arch Linux Post Installation Setup and Configuration Scripts.

These are my personal installation files.

They probably won't work for you.

But you could use them to build your own installation scripts.


## 1. BIOS

Enable UEFI
Disable Secure Boot

--- 

## 2. Boot from USB ISO

---

## 3. Enable WiFi

    $   wifi-menu

---

## 4. Initial Preparation

If terminal font is too small, which can happen if you have a high res display, install terminus fonts:

    $   pacman -S terminus-font

Then set the font to a big one:

    $   setfont ter-v32b

Update font cache

    $   fc-cache -fv

---

## 5. Install Arch base system

Follow the stops in my __[Arch Linux Installation Gude](https://github.com/rickellis/Arch-Linux-Install-Guide)__.

---

## 6. Boot into new installation

    $   sudo wifi-menu

---

Install reflector. First update the databases:

    $   sudo pacman -Sy

    $   sudo pacman -S reflector rsync curl

Now generate mirrorlist:

    $   reflector --verbose --country 'United States' -l 5 --sort rate --save /etc/pacman.d/mirrorlist

---

Initialize the .gitconfig file
    
    git config --global user.name "rickellis"
    git config --global user.email "rickellis@gmail.com"
    git config --global credential.helper cache
    git config --global credential.helper 'cache --timeout=31536000'

---

Clone Archomatic

    $   mkdir /home/CodeLab

    $   git clone https://github.com/rickellis/Archomatic.git

---

Run Archomatic files 01-07. 

Do not install software yet.

---

In order to startx and XFCE we first need to copy the .xinitrc file to home:

    $   cp /home/rickellis/CodeLab/Dotfiles/.xinitrc /home/rickellis/.xinitrc

    $   poweroff

Then reboot

---

Initialize Xorg:

    $   xinit

If it doesn't boot you into XFCE run:

    $   startx

---

Run: 08-software-pacman.sh

---

Run: 09-software-aur.sh

---

Launch Firefox

Then verify the name of the folder where we put chrome so you
can update the personal-setup.sh

---

Run: personal-setup.sh

---

In order for some windows to recognize theme choice we need to copy:

    /home/rickellis/local/share/themes/*

And paste the themes here:

    /usr/share/themes


---

Enable Network Time Protocol to set time using network:

    $   sudo ntpd -qg

---

For larger default terminal fonts create a config file at:

    $   sudo nano /etc/vconsole.config

In it, put this:

    KEYMAP=us
    FONT=ter-v32b

---

Update font cache:

    $   sudo fc-cache -fv   # System fonts

    $   sudo fc-cache -fv ~/.fonts  $ Local fonts

---

Move slimlock themes into:

    /usr/share/slim/themes

Create slimlock config file: 

    /etc/slimlock.conf

Put this in it:

dpms_standby_timeout            60
dpms_off_timeout                600
wrong_passwd_timeout            1
passwd_feedback_x               8%
passwd_feedback_y               54%
passwd_feedback_msg             Authentication failed
passwd_feedback_capslock        Authentication failed (CapsLock is on)
show_username                   0
show_welcome_msg                0
tty_lock                        0
current_theme                   wall

---

Set closing of the laptop lid to suspend:
     sudo nano /etc/systemd/logind.conf
     Change this:
     HandleLidSwitch = suspend

-----------

Firefox: deactivate middle button paste:
     about:config
     middlemouse.paste = false
     middlemouse.opennewwindow = false


-----------

Using 8 cores for MAKEPKG

Open  /etc/makepkg.conf
And set these:

MAKEFLAGS="-j$(nproc)"
COMPRESSXZ=(xz -c -T 8 -z -)

-----------

To solve the giant cursor problem, comment out inherits=theme:

    $ sudo nano /usr/share/icons/default/index.theme

[Icon Theme]
#Inherits=Theme

-----------

MariaDB SETUP

sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql

systemctl enable mysqld

COMMANDS

systemctl start mysqld

systemctl start mysqld

systemctl status mysqld

CHANGE ROOT PASSWORD:

First start mysql, then:

mysql_secure_installation


-----------

PHP SETUP


EDIT: /etc/httpd/conf/httpd.conf

Find the following line and COMMENT IT OUT:

#LoadModule mpm_event_module modules/mod_mpm_event.so


Then, add the following lines at the bottom:

LoadModule mpm_prefork_module modules/mod_mpm_prefork.so
LoadModule php7_module modules/libphp7.so
AddHandler php7-script php
Include conf/extra/php7_module.conf

Then run:  systemctl restart httpd


EDIT: /etc/php/php.ini

Uncomment this:
extension=mysqli

Add these:
extension=bz2.so
extension=mcrypt.so
