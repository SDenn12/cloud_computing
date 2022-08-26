## Amazon Machine Image

### What is an AMI?

An AMI provides the details needed for an instance to be launched in AWS. They are used to save money as a snapshot is taken of an active instance and then saved. This means you can now terminate the current instance. It is not free however is much cheaper than having a running/stopped AWS instance.

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
4. Now set the environment variable by using `echo "DB_HOST=mongodb://DatabaseIPv4Address:27017/posts" | sudo tee -a /etc/environment`
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