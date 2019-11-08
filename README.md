![picture](dsvpn-logo.png)

# Setup Peer to peer secure tunnel with DSVPN

This guide show you my installation setps to setup a DSVPN server on my Azure VPS and a client on my local machine.

## Server

### Make dsvpn
```
sudo apt install build-essential  
git clone https://github.com/jedisct1/dsvpn.git  
cd dsvpn  
make  
```

### Generate shared key
```
dd if=/dev/urandom of=dsvpn.key count=1 bs=32  
```

## Create service
```
sudo cp dsvpn /usr/local/bin  
sudo cp dsvpn.key /usr/local/etc  
sudo vi /etc/systemd/system/dsvpn.service  
```
> Copy below content.
>  
>> [Unit]  
>> Description=DSVPN  
  
>> [Service]  
>> ExecStart=/usr/local/bin/dsvpn server /usr/local/etc/dsvpn.key  
>> Restart=always  
>> RestartSec=30  
  
>> [Install]  
>> WantedBy=network.target  

```
sudo systemctl enable dsvpn.service  
sudo systemctl start dsvpn.service  
```

## Client

### Make dsvpn
```
sudo apt install build-essential  
git clone https://github.com/jedisct1/dsvpn.git  
cd dsvpn  
make  
```

### Copy dsvpn.key from the server

```
scp <user>@<server-public-ip>:/usr/local/etc/dsvpn.key .
```

### Run DSVPN

```
sudo ./dsvpn client dsvpn.key <server-public-ip>  
```

> Use Ctrl-C or `sudo pkill dsvpn` to stop dsvpn.  

### Setup DNS

```
sudo vi /etc/systemd/resolved.conf  
```

> Set below settings. 
>> [Resolve]  
>> DNS=8.8.8.8 8.8.4.4  
>> Domains=~.  

```
sudo systemctl restart systemd-resolved  
```
