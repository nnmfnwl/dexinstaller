### Example of automatization of full dexsetup environment installation 
  * The specific combination of installer arguments used as command installs or updates or forces every component of decentralized exchange system to be set to default expected stage.

#### Summary
  * download installer itself anonymously and run as full automatic installer
  * every installation and runtime component protected by kernel isolation
  * every installation and runtime component protected by tor network isolation
  * detect if tor is already configured
  * detect if to use sudo or su
  * update and install operating system base packages
  * configure vnc server and auto-start user GUI VNC session
  * download dexsetup.installer anonymously by tor
  * run dexsetup.installer with pre-configured arguments
  * download dexsetup.framework
  * configure proxychains
  * download and build all wallets from source
  * generate all predefined wallet profiles
  * BlockDX
  * DEXBOT
  * All trading startegies
  * generate screen script
  * install tor browser and configure tor profile named default
  * install session privacy messenger and configure session profile named default

#### Notice
  * 1. installer will ask for root or sudo password to update system and configure VNC session for user.
  * 2. installer will ask for users new VNC password, but it could be specified as `vncpasswd "password"` installer.sh argument.

#### Command
```
# set base packages for anonymity from very beginning because we do not want even to gitbub to spy on us.
pkgs="proxychains4 tor torsocks wget";

# detect if tor is configured for user or not
groups | grep debian-tor > /dev/null && cfg_user_tor="echo 'Tor for ${USER} is already configured'" || cfg_user_tor="usermod -a -G debian-tor ${USER}";

# detect if to use sudo or su
sudo -v; (test $? != 0) && su_cmd="echo 'Please enter ROOT password'; su - -c" || su_cmd="echo 'Please enter ${USER} sudo password'; sudo sh -c";

# do necessary system update and install all needed packages
eval "${su_cmd} \"apt -y update; apt -y full-upgrade; apt -y install ${pkgs}; ${cfg_user_tor}; exit\""

#download installer and verify file checksum
mkdir -p ~/dexsetup && cd ~/dexsetup && rm -f installer.sh && proxychains4 wget "https://github.com/nnmfnwl/dexinstaller/raw/refs/heads/dev.2025.10.23/installer.sh" && sha512sum installer.sh | grep 63a6802ec2ae7f24913d1f0ff785d39a6192b795fbc490c32fd7c57726bba6d0a84ff807c056fcfa1116ce9a5756a44baf15a71b8789bf63b3efd7fb8d57e016 && (echo "installer fingerprint verification success") || (sha512sum installer.sh; echo "installer fingerprint verification failed"; rm -f installer.sh)

# installer argument DEFAULT-Y is used to automatically answer yes to install or update or forces every component of decentralized exchange system to be set to default expected stage
cd ~/dexsetup && bash installer.sh DEFAULT-Y
```

#### Thanks for reading, feedback is welcome.
  * [Contact me](https://github.com/nnmfnwl/dexinstaller#8-contact-me)
