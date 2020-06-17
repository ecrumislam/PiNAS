# PiNAS
Pi server with Pihole, NAS(DLNA,SAMBA), Torrent Server + WIFI AP


Follow the commands for raspbian lite

Pi
#upgrade

sudo apt-get update && sudo apt-get dist-upgrade -y

#pi-hole

wget -O basic-install.sh https://install.pi-hole.net

sudo bash basic-install.sh

sudo pihole -a -p

#Automount usb

sudo blkid

sudo nano /etc/fstab
#add#

UUID=4072F4F672F4F190  /media/USB/  auto nosuid,nodev,nofail,x-gvfs-show 0 0

sudo mount -a

#miniDlna

sudo apt-get update --fix-missing

sudo apt-get install minidlna -y

sudo nano /etc/minidlna.conf                              > media_dir=drive path
							  > friendly_name=RASPI MINIDLNA
							  
sudo service minidlna restart

sudo service minidlna force-reload

#samba#

sudo apt install samba samba-common-bin smbclient cifs-utils -y

sudo nano /etc/samba/smb.conf >> At the end of the file,
									[Pi]
										path = /home/pi/shared  "Share directory location" 
										read only = no
										public = yes
										writable = yes
										
								
#transmission#

sudo apt-get update && sudo apt-get install transmission-daemon -yes

sudo systemctl stop transmission-daemon

sudo nano /etc/transmission-daemon/settings.json >> 
                                                    download_dir
                                                    incomplete-dir-enabled
                                                    incomplete-dir
                                                    rpc-password
                                                    rpc-username
                                                    rpc-whitelist
                                                    
                                                    
                                                    
sudo systemctl start transmission-daemon

  sudo reboot
 
#UNBOUND# 

sudo apt install unbound -y

wget -O root.hints https://www.internic.net/domain/named.root

sudo mv root.hints /var/lib/unbound/
  
/etc/unbound/unbound.conf.d/pi-hole.conf      >>https://docs.pi-hole.net/guides/unbound/


sudo service unbound start


dig pi-hole.net @127.0.0.1 -p 5335

