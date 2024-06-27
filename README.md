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

In order to get a valid SSL certificate we need to register a new domain name, we can do it using any Domain name registrar - a company that manages reservation of domain names. The most popular ones are: <a href="Godaddy.com">Godaddy.com</a>, Domain.com, Bluehost.com.









