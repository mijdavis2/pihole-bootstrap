# pihole-bootstrap
Setting up raspberry pi, setting up pihole, etc etc.

**Features**

- headless rpi
- remote access via ssh
- fish shell
- pihole

## RPi OS Setup

- Download headless version of raspbian via https://www.raspberrypi.org/downloads/raspbian/ (currently referred to "Raspbian Buster Lite")
- Follow instructions here: https://www.raspberrypi.org/documentation/installation/installing-images/README.md (currently includes using etcher, which is a GREAT tool: https://www.balena.io/etcher/)

## SSH setup

- While the drive is still in your machine, open the boot drive and `touch ssh`. This will allow for ssh access.

## WiFi

If you require wifi (i.e. no ethernet connection to your pi), use this: https://linuxhint.com/raspberry_pi_headless_mode_ubuntu/

## Login and setup

- If you have access to your router, find the IP address of the pi. Otherwise, use the link above to nmap and find it.
- Run `ssh pi@<ip address>`
- Password will be `rasbperry`

## Fish (shell)

- Run `sudo apt-get update; sudo apt-get install -yq fish`
- Run `chsh -s \`which fish\``
- `fish`
- Add update function:
```function fupdate
    sudo apt-get update; sudo apt-get upgrade --with-new-pkgs -y; sudo apt-get autoremove -y
end```
- Run `fupdate`
