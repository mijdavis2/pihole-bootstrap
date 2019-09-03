# pihole-bootstrap
Setting up raspberry pi, setting up pihole, etc etc.

**Features**

- headless rpi
- remote access via ssh
- fish shell
- pihole

## Hardware reqs

- Get yourself a raspberry pi. I suggest this kit: https://www.amazon.com/gp/product/B07BC7BMHY/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1
- An SD card like this (assuming compatibility with rpi you use is similar to above): https://www.amazon.com/gp/product/B00WR4IJBE/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1
- Ethernet cord to plug it into your router.

> You can get JUST a pi and a smaller microSD card and use something like LEGOs to make yourself a housing. Google is your friend here. Just be sure to get a microSD to SD or USB converter so you can plug it into your main machine and install the rpi OS.

**These instructions are for installing with linux as your main machine... but it won't be very different for OSX or Windows**.

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
- To change password, run: `passwd`
- Save your password to a SECURE password manager like lastpass or keypass or credstash.
- `exit` and ssh in again to check your new password works before we start making more changes!

## Essentials

- Run `sudo apt-get update; sudo apt-get install -yq fish`
- Run `chsh -s \`which fish\``
- `fish`
- Add update function:
```
function fupdate
    sudo apt-get update; sudo apt-get upgrade --with-new-pkgs -y; sudo apt-get autoremove -y
end
```
- Run `fupdate`
- Run `sudo apt-get install -yq git`

## Docker

- Run 
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```
- RESTART the pi to get docker working properly!
- To add pi user to docker group (not necessarily recommended):
```
sudo usermod -aG docker $USER
```

## Pi-Hole

- Use my docker-pi-hole fork: https://github.com/mijdavis2/docker-pi-hole
- Once pihole is installed and running via docker, reset the password via:
```
docker exec -it pihole_container_name pihole -a -p
```
- Save the password somewhere secure like lastpass, keypass, or credstash.

## Router

**Add static IP to rpi**

- Login to your router's admin console
- Go to something like "Address Reservation" (likely under DHCP settings)
- Using the rpi MAC address, reserve a specific IP for your pi to be fixed to
- Save and reboot router
- Reboot the pi
- Make sure the pi comes up with the IP you reserved for it; you can test by going to that IP in your browser to access the pi-hole console. e.g. http://192.168.0.99/admin.

**Use pihole as DNS"

- Find DNS settings (likely again under DHCP settings)
- Set the primary DNS to the IP of the rpi
- Set the secondary to `0.0.0.0`
- Save and reboot the router

## Update blacklist

- I found a community blacklist via reddit:
    - The original post: https://old.reddit.com/r/pihole/comments/8cjle5/popular_block_lists/
    - Direct link to blacklist: https://firebog.net/
- Go to pihole admin page and login
- Go to **Settings > Blacklists**
- Paste the text list of blacklists you found above into the blacklists input
- Save and update the pihole

## Other config

- There's a suggested whitelist on the firebog.net site. You can add those to the Whitelist settings.
- Set DNS on the pihole itself to select between different DNS providers
- Enable DNSSEC for more even more security

## Enjoy

- Enjoy ad-free and secure web browsing!
