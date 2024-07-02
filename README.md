# Load Balancer Solution With Nginx and SSL/TLS
A Load Balancer (LB) distributes clients' requests among underlying Web Servers and makes sure that the load is distributed in an optimal way. 
In this project, we will configure an Nginx Load Balancer Solution.

It is extremely important to ensure that connections to our Web Solutions are secure and information is encrypted in transit. 
Connection over secured HTTP (HTTPS protocol), it's purpose and what is required to implement it will be covered.

## Task
This project consist of two parts:

1. Configure Nginx as a Load Balancer
2. Register a new domain name and configure secure connection

The diagrame below shows the architecture of the solution

<img width="788" alt="architecture" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/b1bb0b1d-6e9d-4c08-89ca-66d18249beb6">

### Part 1 - Configure Nginx As A Load Balancer
#### 1. Create an EC2 VM based on Ubuntu Server 24.04 LTS and name it nginx LB

Result:

<img width="1417" alt="Screenshot 2024-06-27 at 16 40 05" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/685c6246-71dc-4174-812f-61eb40b35645">

<img width="1664" alt="Screenshot 2024-06-27 at 16 41 16" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/a122171b-d59a-4274-bb40-4f4a42046e4b">

#### 2. Open TCP port 80 for HTTP connections and TCP port 443 for secured HTTPS connections

Result:

<img width="1640" alt="Screenshot 2024-06-27 at 16 42 26" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/9000ef33-85b3-4c31-b17d-5c6e543d5aed">

#### 3. Update /etc/hosts file for local DNS with Web Servers' names (e.g web1 and web2) and their local IP addresses

- Access the instance

```
ssh -i ~/Downloads/demo-pair.pem ubuntu@34.230.70.9
```

Result:

<img width="784" alt="Screenshot 2024-06-27 at 16 43 52" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/bfd4eb8d-950b-4a3e-ad18-bcc1fd640a4a">

- Update the hosts file

```
sudo vi /etc/hosts
```

Result:

<img width="817" alt="Screenshot 2024-06-27 at 16 45 46" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/d3b645ae-9367-464e-ab07-6267addb1497">

#### 4. Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers

 - Update the instance

```
sudo apt update && sudo apt upgrade -y
```

Result:

<img width="1023" alt="Screenshot 2024-06-27 at 16 46 40" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/43cce912-c1ce-4050-b848-29d3ff6bc7a2">

- Install Nginx

```
sudo apt install nginx
```

Result:

<img width="1239" alt="Screenshot 2024-06-27 at 16 49 04" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/c9d55a83-130d-4ac3-82c0-93dfd421b09b">

#### 5. Configure Nginx LB using the Web Servers' name defined in /etc/hosts

- Open the default Nginx configuration file

```
sudo vi /etc/nginx/nginx.conf
```

Insert the following configuration in http section

```
    upstream myproject {
       server Web1 weight=5;
       server Web2 weight=5;
    }

    server {
        listen 80;
        server_name www.domain.com;

        location / {
            proxy_pass http://myproject;
        }
    }
    # comment out this line
    # include /ete/nginx/sites-enabled/
```

Result:

<img width="841" alt="Screenshot 2024-06-27 at 16 54 44" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/101eaf22-703e-449d-ae6c-c58a893efc71">

- Test the server configuration

```
sudo nginx -t
```

Result:

<img width="622" alt="Screenshot 2024-06-27 at 16 56 11" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/522c4d8c-1f2e-4957-88c6-700b8d8456af">

- Restart Nginx and ensure the service is up and running

```
sudo systemctl restart nginx
sudo systemctl status nginx
```

Result:

<img width="1004" alt="Screenshot 2024-06-27 at 16 57 01" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/aa283949-5512-456a-8e04-188f81064725">

## Part 2 - Register a new domain name and configure secured connection using SSL/TLS certificates

In order to get a valid SSL certificate we need to register a new domain name, we can do it using any Domain name registrar - a company that manages reservation of domain names. The most popular ones are: <a href="Godaddy.com">Godaddy.com</a>, <a href="Domain.com">Domain.com</a>, <a href="Bluehost.com">Bluehost.com</a>.

#### 1. Register a new domain name with any registrar of your choice in any domain zone. (e.g .com, .net, .org, .edu, info, .xyz or any other)

- <a href="Cloudns.net">Cloudns.net</a> is the domain name registrar used for this project.

Results:

<img width="1609" alt="Screenshot 2024-06-27 at 17 03 53" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/74fa40ba-27e7-425b-9bbd-24274b6a29ad">

<img width="1661" alt="Screenshot 2024-07-02 at 00 55 05" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/3a5ce47d-3779-4e35-bd8a-7e3be96280d4">


#### 2. Assign an Elastic IP to our Nginx LB server and associate our domain name with this Elastic IP
This is neccessary in order to have a static IP address that does not change after reboot.

Results:

<img width="1680" alt="Screenshot 2024-07-02 at 01 04 28" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/4f1666b1-6b59-4e96-8f4e-d061b759a363">


<img width="1216" alt="Screenshot 2024-07-02 at 01 06 45" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/4cd35b88-e4fa-4a22-9ae6-1605fc381982">


<img width="1660" alt="Screenshot 2024-07-02 at 01 08 09" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/d6998866-bd70-477f-9791-bc1cb379a748">


<img width="1436" alt="Screenshot 2024-07-02 at 01 08 57" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/11c4246a-1cfe-4e01-8e30-31939545155b">

#### 3. Update or create A record your registrar to point to Nginx LB using the elastic IP

Result:

<img width="1531" alt="Screenshot 2024-07-02 at 01 12 32" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/299f9609-0797-4c48-a24c-cbb6d0dafb14">

<img width="1522" alt="Screenshot 2024-07-02 at 01 13 35" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/fcda7ea4-7db8-4377-9ebc-250925f80995">

Use <a href="https://dnschecker.org/#A/www.toolingsolution.dns-dynamic.net">nds checker</a> to Verify the DNS record

Result:

<img width="1337" alt="Screenshot 2024-07-02 at 01 19 02" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/6d5f6a30-e01f-4452-84ab-e4fee7bf4257">


#### 4. Configure Nginx to recognize your new domain name

Update your nginx.conf with server_name www.<your-domain-name.com instead of server_name www.domain.com

In our case, the server_name is www.devopsnew.dns-dynamic.net

```
sudo vi /etc/nginx/nginx.conf
```

 Result:

 <img width="802" alt="Screenshot 2024-07-02 at 01 22 40" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/f240893b-4679-447b-a1b2-ce4edf56c6d7">


 #### Restart and check status of Nginx

 ```
sudo systemctl restart nginx
sudo systemctl status nginx
```

Result:

<img width="1042" alt="Screenshot 2024-07-02 at 01 23 56" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/d51d21ce-4665-4584-a22e-3ec547541818">

#### Check that the Web Server can be reach from a browser with the new domain name using HTTP protocol.

```
http://<your-domain-name.com>
```

Result:

<img width="1638" alt="Screenshot 2024-07-02 at 01 25 40" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/6d59cc5c-2d00-4bad-8029-c1d681826696">


#### 5. Install <a href="https://certbot.eff.org/">certbot</a> and request for an SSL/TLS certificate

Ensure <a href="https://snapcraft.io/snapd">snapd</a> service is active and running

```
sudo systemctl status snapd
```

Result:

<img width="1198" alt="Screenshot 2024-07-02 at 01 29 44" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/1ea4aaa4-dc80-470c-a5a3-2281fb610618">

#### Install certbot

```
sudo snap install --classic certbot
```

Result:

<img width="792" alt="Screenshot 2024-07-02 at 01 31 06" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/52b3b1c4-9f0f-49c1-b410-86cf9a275810">

### Request SSL/TLS Certificate

Create a Symlink in /usr/bin for Certbot: Place a symbolic link in this PATH to make it easier to run certbot from the command line without needing to specify its full path.

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

Follow the certbot instructions you will need to choose which domain you want your certificate to be issued for, domain name will be looked up from nginx.conf file so ensure you have updated it on step 4.

```
sudo certbot --nginx  # Obtain certificate
```

Result:

<img width="847" alt="Screenshot 2024-07-02 at 01 36 24" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/1f1aeddc-6597-4f56-887f-6010a7a1c9e5">

### Test secured access to your Web Solution by trying to reach https://<your-domain-name.com>.

You shall be able to access your wesite using HTTPS protocol (Uses TCP port 443) and see a padlock image in your browsers' search string. Click on the padlock icon and you can see the detail of the certificate issued for the website.

Results:

<img width="1471" alt="Screenshot 2024-07-02 at 01 38 20" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/67278922-2887-4d32-bfe2-d2df09122628">

<img width="1280" alt="Screenshot 2024-07-02 at 01 38 36" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/e628a7e9-9b49-4869-933d-82ad569265b4">

<img width="1548" alt="Screenshot 2024-07-02 at 01 40 03" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/df093911-de0f-4425-ad94-71d122e7a98b">

#### 6. Set up periodical renewal of your SSL/TLS certificate

By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently.

Test the renewal command in dry-run mode

```
sudo certbot renew --dry-run
```

Result:

<img width="833" alt="Screenshot 2024-07-02 at 01 41 49" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/fbe9ead8-ea4a-40c6-a57c-26213b566ff2">

Best pracice is to have a scheduled job that runs renew command periodically. Configure a cronjob to run the command twice a day

Edit the crontab file

```
crontab -e
```

Result:

<img width="707" alt="Screenshot 2024-07-02 at 01 43 09" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/e2e27a16-d804-47da-b8f8-1e30abdaf7c8">

Add the following line to scheduled a job that runs renew command twice daily

```
* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1
```

Result:

<img width="938" alt="Screenshot 2024-07-02 at 01 43 39" src="https://github.com/sheezylion/load-balancer-solution-with-nginx-/assets/142250556/753e5107-d5f9-41ef-b42c-a26b69a68a26">

You can always change the interval of the cronjob if twice a day is too often by adjusting the schedule expression.






