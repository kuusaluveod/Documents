# samba jaoks eraldi kasutaja
$ sudo adduser username

# install samba
$ sudo apt install tasksel
$ sudo tasksel install samba-server

# conf backup

$ sudo cp /etc/samba/smb.conf /etc/samba/smb.conf_backup
$ sudo bash -c 'grep -v -E "^#|^;" /etc/samba/smb.conf_backup | grep . > /etc/samba/smb.conf'

# samba kasutaja -> 2x parool
sudo smbpasswd -a username

# ja lisa etc/samba/smb.conf lõppu ->
[homes]
   comment = Home Directories
   browseable = yes
   read only = no
   create mask = 0700
   directory mask = 0700
   valid users = %S

# anonüümne read-write. tee dir ja muuda permission
$ sudo mkdir /var/samba
$ sudo chmod 777 /var/samba/

# ja lisa etc/samba/smb.conf lõppu ->
[public]
  comment = public anonymous access
  path = /var/samba/
  browsable =yes
  create mask = 0660
  directory mask = 0771
  writable = yes
  guest ok = yes

#samba restart

$ sudo systemctl restart smbd
