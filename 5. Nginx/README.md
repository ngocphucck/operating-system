1. Install nginx: ```$ sudo apt-get install nginx```
2. Create a simple `index.html` file in `/var/www/html`.
3. Config file `/etc/nginx/nginx.cfg` with the following content:
```
user usr;

http {
	server {
		listen 80;
		server_name example.vn;
		
		root /var/www/html;
	}

}

events {}

```
4. Remember to open port 80 in your device, if not read the `iptables usage`.
5. Reload: ```$ sudo systemctl reload nginx```
If failed, check: ```$ sudo systemctl status nginx```
If not active, run: ```$ sudo systemctl start nginx```
6. Open `http://localhost:80/`
7. Upgrade our website to `https`:
- Create CSR request file `csr.req.txt`:
    ```
    [req]
    default_bits = 2048
    prompt = no
    default_md = sha256
    req_extensions = req_ext
    distinguished_name = dn

    [ dn ]
    C=VN
    ST=H
    L=H
    O=X
    OU=Company
    emailAddress=webmaster@x.com
    CN = *.aimesoft.com

    [ req_ext ]
    subjectAltName = @alt_names

    [ alt_names ]
    DNS.1 = x.com
    ```

- Create a private key and CSR file: ```$ openssl req -new -newkey rsa:2048 -nodes -keyout ssl.key -out ssl.csr -config csr.req.txt```
- Crate CSR certificate: ```$ openssl x509 -req -days 365 -in ssl.csr -signkey ssl.key -out ssl.crt```
- Config `nginx.cfg`: 
    ```
    user usr;

    http {
        server {
            listen 80;
            listen 443 ssl;
            server_name example.vn;
            
            root /var/www/html/;
            ssl_certificate     path_to_csr;
            ssl_certificate_key path_to_key;
        }

    }

    events {}

    ```
- Reload nginx: ```$ sudo systemctl reload ngix```
- Connect to the address in web browser: ```$ https://localhost:443/```
8. We can use `proxy` to mapping ports.
