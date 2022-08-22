# Cloud Computing

## What is Cloud Computing?

The practice of using a network of remote servers hosted on the internet to store, manage, and process data, rather than a local server or a personal computer.

### What are the benefits of Cloud Computing?

- High speed – quick deployment
- Automatic software updates and integration CI-CD
- Efficiency and cost reduction
- Scalability
- Collaboration
- Data Loss Prevention

### What is IaaS?

- Infrastructure as a Service
- Fastest and least expensive
- Management of network, servers and data storage on the cloud 
- Examples: AWS, Microsoft Azure, Google Cloud, IBM Cloud

### What is PaaS?

- Platform as a Service
- PAAS products allow businesses and developers to host build and deploy consumer-facing apps.
- Google App Engine, Red Hat Openshift, Heroku, Apprenda

### What is SaaS?

Software as a Service (instead of packaged software)
Most common cloud service
Offer consumers and businesses cloud-based tools and applications for everyday use

### Things to Consider when Looking at Creating a Remote VM

- Processing power
- OS 
- Storage
- Power Supply
- Cost

The uses of the VM must be considered when selecting specifications as limiting cost is relevant.

## How to Setup AWS Application

![](image/diagram.png)

### 1. Create a VM on AWS

1. Select the Operating System
![](image/selectvm.PNG)

2. Select the Processing Power
![](image/select_instance.PNG)

3. Configure instance details
![](image/select_confinstane3.PNG)

4. Select the storage
![](image/selectstore4.PNG)

5. Attach tags
![](image/selecttag5.PNG)

6. Select which ports should be allowed to interact with the VM
![](image/selectfirewall6.PNG)

### 2. SSH Connect to VM

![](image/ssh_connect.PNG)

- In the location of the key enter the command at the bottom to SSH into the VM
 
### 3. Install dependencies

```
# updates ubuntu
sudo apt-get update
sudo apt-get upgrade -y

# nginx install
sudo apt-get install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx

# nodejs install
sudo apt-get purge nodejs npm
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install -y npm

# pm2 install
sudo npm install pm2 -g
```

### 4. Copy files from Local Host to Remote Host

- SCP Command (used to migrate from lh to gh) `scp -i [key_name] -r [file_source] [file_destination] ` 
- The file destination is formatted as @user:PUBLIC_DNS so in this example it uses `ubuntu@ec2-34-240-115-36.eu-west-1.compute.amazonaws.com`

### 5. Reconfigure NGINX to allow reverse proxy

1. `sudo nano /etc/nginx/sites-available/default`
2. Reconfigure the file as such
```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html;

        
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                proxy_pass http://localhost:3000;
        }

}
```
3. `sudo systemctl restart nginx`

### 6. Run NPM start and the app should be working
- Using the public IP address found,
![](image/public_address.PNG)
- The app should now be working
![](image/appworking.PNG)

## Database Extension

### Set up Database VM

- Same configuration as app vm, except for security protocols which look like 

![](image/security_prot_db.PNG)

### Install dependencies on DB VM.

```
# updates ubuntu
sudo apt-get update
sudo apt-get upgrade -y

# retrieves key from mongodb
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv D68FA50FEA312927
echo "deb https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

# updates ubuntu
sudo apt-get update
sudo apt-get upgrade -y

sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20

# enables and starts mongodb
sudo systemctl restart mongod
sudo systemctl enable mongod
```

Now configure the mongod.conf file such that the `bind ip` is `0.0.0.0`

Reset mongod so the configuration can be made live `sudo systemctl restart mongod`

### Reconfiguring the App VM

- Set a environment variable called DB_HOST and set equal to `mongodb://IPv4_db_address/27017/posts`
- Run `node seed.js` in seeds directory (inside app directory)
- Run `npm start` to start the app

## Global Infrastructure

An availibility zone is a data centre, which is located within a region. The purpose of these is to save data if an availbility zone was to go down.

## The 6 Pillars of the AWS Well-Architected Framework (from AWS)

### Operational Excellence

- Perform operations as code
- Make frequent, small, reversible changes
- Refine operations procedures frequently
- Anticipate failure
- Learn from all operational failures

### Security 

- Implement a strong identity foundation
- Enable traceability
- Apply security at all layers
- Automate security best practices
- Protect data in transit and at rest
- Keep people away from data
- Prepare for security events

### Reliability

- Automatically recover from failure
- Test recovery procedures
- Scale horizontally to increase aggregate workload availability
- Stop guessing capacity
- Manage change in automation

### Performance Efficiency

- Democratize advanced technologies
- Go global in minutes
- Use serverless architectures
- Experiment more often
- Consider mechanical sympathy

### Cost Optimization

- Implement cloud financial management
- Adopt a consumption model
- Measure overall efficiency
- Stop spending money on undifferentiated heavy lifting
- Analyze and attribute expenditure

### Sustainability

- Understand your impact
- Establish sustainability goals
- Maximize utilization
- Anticipate and adopt new, more efficient hardware and software offerings
- Use managed services
- Reduce the downstream impact of your cloud workloads

### Provisioning in User data

This can be configured in the instance settings.

```
#!/bin/baash

sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install nginx
```

You can edit at a later date using:

![](image/user_data.PNG)

## Amazon Machine Image

### What is an AMI?

### What are the benefits of an AMI?

## Implementing AMI into Node App

1. Create app instance

Enter provisioning script into user data
```
#!/bin/bash

# nginx install
sudo apt-get install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx

# nodejs install
sudo apt-get purge nodejs npm
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install npm -y
```
2. Create DB instance using the APP private ipv4 in the security group

Enter provisioning script into user data

```
# updates ubuntu
sudo apt-get update
sudo apt-get upgrade -y

# retrieves key from mongodb
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv D68FA50FEA312927
echo "deb https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

# updates ubuntu
sudo apt-get update
sudo apt-get upgrade -y

sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20

# enables and starts mongodb
sudo systemctl restart mongod
sudo systemctl enable mongod
```
3. Now SSH into the DB machine and set `bind ip : 0.0.0.0` in the `/etc/mongod.conf` file. After this run `sudo systemctl restart mongod` to update your configuration.
4. Now set the environment variable by using `echo "DB_HOST=mongodb://192.168.10.150:27017/posts" | sudo tee -a /etc/environment`
`
5. Now SSH into the APP machine and configure the reverse proxy in the `/etc/nginx/sites-available/default` as done before in the manual example.
6. Inside the app machine locate the folder with app.js and run `npm install` then enter the `/seeds` directory and run `node seed.js`.
7. Go back one directory using `cd ..` then run `npm start`. 
8. Your app should now be running on the public ipv4 address.

### Rebooting after Termination
1. Boot APP instance
2. Boot DB instance changing the security group to the APP IP.
3. SSH into APP instance, change DB_HOST to incorporate the new DB IP, complete the `node seed.js` in the seeds directory and then `npm start` in the app.js folder.
4. The app should now be working.


![](image/ami_diagram.png)