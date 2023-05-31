# Kasm Workspaces with Automated HTTPS from Caddy2

This repository contains the required components in order to build a fully functional dockerized installation of Kasm and Caddy2.

## Requirements
Install `docker` and `docker-compose`:

```bash
curl -fsSL https://get.docker.com | sh - && \
sudo usermod -aG docker $USER && \
sudo apt-get install libffi-dev libssl-dev -y && \
sudo apt install python3-dev -y && \
sudo apt-get install -y python3 python3-pip && \
pip3 install docker-compose 
```
## Instructions to setup

### 1. Create a DNS record that points to the IP of the machine.
This is required for the Caddy2 server to obtain the Let's Encrypt certificates.

### 2. Modify the Caddyfile to the correct DNS record.

```bash
mv caddy/Caddyfile.example caddy/Caddyfile
```

Inside `Caddyfile`, change `domain.example.com` by your domain `mydomain.com`:

```
domain.example.com {
        reverse_proxy kasm:8443 {
                transport http {
                        tls_insecure_skip_verify
                }
        }
}
```

### 3. Run the docker-compose file

``` 
docker-compose up -d

````
### 4. Configure Kasm
Access the address on port `3000` and configure Kasm. The address should be [https://mydomain.com:3000](https://mydomain.com:3000).

### 5. Modify the Kasm Zones to:

- `Upstream Auth Address: mydomain.com` # The domain you chose before.
- `Proxy Port: 0`


### 6. Enjoy your new installation of Kasm now without needing the port
[https://mydomain.com](https://mydomain.com)

