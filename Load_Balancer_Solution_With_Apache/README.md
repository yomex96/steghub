# Load Balancer Solution with Apache

## Overview

This project demonstrates deploying and configuring an Apache Load Balancer to serve a Tooling Website solution on a separate Ubuntu EC2 instance. The purpose of this load balancer is to distribute traffic evenly across multiple web servers, enhancing fault tolerance, scalability, and availability. For simplicity, this setup includes two web servers, but the configuration can be extended to three or more servers as needed.

## Task
Deploy and configure an Apache Load Balancer on an Ubuntu EC2 instance that enables users to access the web application via the load balancer, which then distributes incoming traffic to the web servers.

## Prerequisites

An AWS account with permissions to create and configure EC2 instances.
Basic knowledge of Apache, Linux, and AWS EC2.
SSH access to the EC2 instances.

## Architecture
## Load Balancer (Apache): Distributes incoming requests to multiple web servers.
## Web Servers: Hosts the Tooling Website and responds to requests from the load balancer.
## The Apache Load Balancer directs user traffic to available web servers, ensuring that the website remains accessible even if one of the web servers becomes unavailable.





## Author : Abayomi Robert Onawole
