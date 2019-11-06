![picture](dsvpn-logo.png)

# Work in process...

## Make dsvpn
```
git clone https://github.com/jedisct1/dsvpn.git  
cd dsvpn  
make  
```

## Generate shared key
```
dd if=/dev/urandom of=dsvpn.key count=1 bs=32  
cat dsvpn.key | base64  
```

## Run on server
```
dsvpn server dsvpn.key auto  
```

## Run on client
```
dsvpn client dsvpn.key <server-public-ip>  
```

