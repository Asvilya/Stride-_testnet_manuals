<p style="font-size:14px" align="right">
<a href="https://kjnodes.com/" target="_blank">Visit our website <img src="https://user-images.githubusercontent.com/50621007/168689709-7e537ca6-b6b8-4adc-9bd0-186ea4ea4aed.png" width="30"/></a>
<a href="https://discord.gg/QmGfDKrA" target="_blank">Join our discord <img src="https://user-images.githubusercontent.com/50621007/176236430-53b0f4de-41ff-41f7-92a1-4233890a90c8.png" width="30"/></a>
<a href="https://kjnodes.com/" target="_blank">Visit our website <img src="https://user-images.githubusercontent.com/50621007/168689709-7e537ca6-b6b8-4adc-9bd0-186ea4ea4aed.png" width="30"/></a>
</p>

<p style="font-size:14px" align="right">
<a href="https://hetzner.cloud/?ref=y8pQKS2nNy7i" target="_blank">Deploy your VPS using our referral link to get 20€ bonus <img src="https://user-images.githubusercontent.com/50621007/174612278-11716b2a-d662-487e-8085-3686278dd869.png" width="30"/></a>
</p>

<p align="center">
  <img height="100" height="auto" src="https://user-images.githubusercontent.com/50621007/169664551-39020c2e-fa95-483b-916b-c52ce4cb907c.png">
</p>

# Set up ping pub for your cosmos chains

## Set up vars
```
CHAIN_NAME=dws
API_PORT=14317
```

## Update packages
```
sudo apt update && sudo apt upgrade -y
```

## Install nginx and certbot
```
sudo apt install nginx certbot python3-certbot-nginx -y
```

## Set up config
```
sudo tee /etc/nginx/sites-enabled/default > /dev/null <<EOF
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        server_name ${CHAIN_NAME}.api.kjnodes.com;

        location / {

                add_header Access-Control-Allow-Origin *;
                add_header Access-Control-Max-Age 3600;
                add_header Access-Control-Expose-Headers Content-Length;

                proxy_pass http://127.0.0.1:${API_PORT};
        }
}
EOF
```

## Obtain our certificates
```
sudo certbot --nginx -d ${CHAIN_NAME}.api.kjnodes.com --register-unsafely-without-email
```

## Rename default file to match domain
```
mv /etc/nginx/sites-enabled/default /etc/nginx/sites-enabled/${CHAIN_NAME}.api.kjnodes.com.conf
```

## Reload nginx
```
systemctl reload nginx.service
```
