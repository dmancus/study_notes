Setting up OpenVPN Server
================
1. Set up vpn server user and client user  
```
sudo adduser openvpn - (set up password for user and store it somewhere)
sudo adduser openvpn sudo - add this new user to the sudoers file
sudo adduser vpnclient - (set up password for user and store it somewhere)  
Log out and Log back in as openvpn user:  
ssh [-p port] openvpn@<ipaddress>  
```  
2. Download `sudo wget <link from openvpn.net website>`
3. Install `sudo dpkg -i <filename>`
4. URLs to access vpn will be displayed
5. Login from browser and now you can do configs via web UI

Setting up OpenVPN client
==========================
1. Access the url provided right after install for non-admin Access
2. Log in as vpnclient
3. Download the profile configuration file for "Yourself", aka vpnclient user.  This is a .ovpn file
4. Android - Download the openvpn app
5. "Import" the .ovpn file, it must be on filesystem locally somewhere
6. Open vpn connection, provide auth details for vpnclient user and it connects!

Adding SSH to the OpenVPN connection
====================================
TODO
