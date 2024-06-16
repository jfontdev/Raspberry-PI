# Raspberry config

## Network

### Static IP:

- ifconfig -> get the network interface name

- sudo nano /etc/network/interfaces.d/eth0 -> file before debian buster

```
'allow-hotplug eth0'
'iface eth0 inet static'
'address 192.168.1.41'
'network 192.168.1.0'
'netmask' 255.255.255.0
'gateway' 192.168.1.1
```

- sudo nano /etc/dhcpcd.conf -> after debian buster

```
'interface eth0'
'static ip_address=192.168.1.41/24'
'static routers=192.168.1.1'
'static domain_name_servers=192.168.1.1'
```

### SFTP:

- Install vsftpd ->
  `sudo apt install vsftpd`

- Change configuration in the config file ->
  `sudo nano /etc/vsftpd.conf `

- Uncomment or add the following lines to the config file:

  ```
  anonymous_enable=NO
  local_enable=YES
  write_enable=YES
  local_umask=022
  chroot_local_user=YES
  user_sub_token=$USER
  local_root=/home/$USER/FTP
  ```

  (note): Last 2 should be the ones to be added, this, makes it so the user is locked to their home directory within a folder called 'FTP'

- Create the FTP directory -> `mkdir -p /home/<user>/FTP/files`

  (note): By using the -p argument we are telling mkdir that it needs to create the entire path tree.

- Change permission of the new folder -> `chmod a-w /home/<user>/FTP`  
   (note): We remove the write permission for everyone.

- Restart the service to make the new settings take effect -> `sudo service vsftpd restart`

Source: [PiMyLifeUp](https://pimylifeup.com/raspberry-pi-ftp/)

## Temperature

- Get instant cpu temperature -> `vcgencmd measure_temp`
