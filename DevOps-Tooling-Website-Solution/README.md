# DevOps Tooling Website Solution: Multi-Tier Web Application

## Overview

This DevOps project implements a multi-tier web application with the following components:

1. **Infrastructure**: AWS
2. **Web Server**: Red Hat Enterprise Linux 8
3. **Database Server**: Ubuntu 24.04 + MySQL
4. **Storage Server**: Red Hat Enterprise Linux 8 + NFS Server
5. **Programming Language**: PHP
6. **Code Repository**: GitHub

## Architecture

The project uses a multi-tier architecture, with each component on a separate server or service:

+-------------+     +------------------+     +------------------+
| Web Server  |     | Database Server  |     |  Storage Server  |
| (RHEL 8)    |     | (Ubuntu 24.04)   |     |  (RHEL 8 + NFS)  |
|  +-------+  |     |  +-----------+   |     |   +---------+    |
|  |PHP App|  |     |  |  MySQL    |   |     |   |   NFS   |    |
|  +-------+  |     |  +-----------+   |     |   +---------+    |
+-------------+     +------------------+     +------------------+
|                     |                         |
|                     |                         |
+------------------+  +------------------+  +------------------+
|       AWS        |  |       AWS        |  |       AWS        |
|  Infrastructure  |  |  Infrastructure  |  |  Infrastructure  |
+------------------+  +------------------+  +------------------+

## Components

### Infrastructure (AWS)
Hosted on AWS, using services like EC2 and EBS.

### Web Server (RHEL 8)
Runs the PHP application on Apache or Nginx.

### Database Server (Ubuntu 24.04 + MySQL)
Hosts the MySQL database instance.

### Storage Server (RHEL 8 + NFS)
Provides shared storage via NFS.

### Programming Language (PHP)
Used for web application development.

### Code Repository (GitHub)
Manages source code and version control.

## Deployment Workflow

1. **Infrastructure Provisioning**: Set up AWS resources using IaC tools.
2. **Server Configuration**: Configure servers with tools like Ansible or Puppet.
3. **Application Deployment**: Deploy PHP app, configure database, and set up NFS storage.
4. **CI/CD**: Implement automated pipeline for build, test, and deployment.


## Author : Abayomi Robert Onawole 
For detailed setup and deployment instructions, see the project wiki or documentation repository.
