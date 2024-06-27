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
1. Create an EC2 VM based on Ubuntu Server 24.04 LTS and name it nginx LB

