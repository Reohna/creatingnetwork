- Steps on how to set up a network:
	- 1.Create your Network
		- a. Create a Router:
			- Clone "Link Clone" Centos_8_base
			- Set "Host-Only Adapter "
			- Turn on Promiscious Mode
		- b. Assign Network Address:
			- Example: Network Address: 192.168.172.32/27
			- nmtui -> assign "Manual" ip address to the interface, 10.20.30.[Number]/24
				- [Number] is personally assigned
			- Assign "Manual" Ip address to the router (192.168.172.33/27) [
				- Can't be Network Address]
		- c. Restart your interface and Routers in nmtui before continuing
	- 2. Enable IP Forwarding
		- a. sudo vim /etc/sysctl.conf
			- `net.ipv4.ip_forward = 1`
		- b. Activated the changes
			- sudo sysctl --system
	- 3. Edit DHCP file
		- a. `sudo vim /etc/dhcp/dhcpd.conf`
			-
			  ```
			  subnet 192.168.172.32 netmask 255.255.255.224{
			  
			  option routers 192.168.172.33; #This router is not the same as your network Router,
			  
			  range 192.168.172.34 192.168.172.40; #This range should not include your option router or the first / last address of the network address
			  }
			  ```
		- Calculate "netmask" by using https://jodies.de/ipcalc?host=192.168.172.32&mask1=27&mask2=
		- b. Check for Syntax Error
			- `sudo dhcpd - t`
		- c. Start dhcpd service
			- `sudo systemctl start dhcpd.service`
		- d. Enable dhcpd service
			- `sudo systemctl enable dhcpd.service`
	- 4. Routing(bird file)
		-
		- a. Edit Bird Configuration File
			- `sudo vim /etc/bird.conf`
				-
				  ```
				  log sylog all;
				  router id 10.20.30.[number];
				  protocol device{}
				  
				  protocol kernel {
				  	ipv4{
				      	export all;
				      };
				  }
				  
				  protocol ospf {
				  	area 0 {
				      	interface "enp0s3"{};
				          interface "enp0s8"{stub;};
				      };
				  }
				  ```
			- b. Check for Syntax errors in Bird
				- `sudo bird -p`
			- c. Enable / Start the Bird Service
				- `sudo systemctl start bird.service` and ` sudo systemctl enable bird.service`
				-
			-
		-
		-
		-
		- log sylog all;
		- router id 10.20.30.[number];
		- protocol device
		-
		- ```
		-
		-
	-
