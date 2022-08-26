
### Load Balancer

Load balancers balance the traffic between multiple instances, there are different types of load balancers which are: Application Load Balancers, Network Load Balancers.

Network Load Balancers simply forwards requests. It cannot assure availability of the application as it cannot tell the difference between different applications. This is because it works at the Network Level.

The Application Load balancers are able to take much more contextual information however this comes at the cost of speed.

### Autoscaling Group

Attached to load balancers, they create and delete new instances as needed (according to metrics which you wish to monitor).

Simple Policy - Increase or decrease the current capacity of the group based on a single scaling adjustment.

Step Policy - Increase or decrease the current capacity of the group based on a set of scaling adjustments, known as step adjustments, that vary based on the size of the alarm breach.

Target tracking - Increase or decrease the current capacity of the group based on a target value for a specific metric.

### Route 53 DNS

Reroutes all of the traffic from different instances to the same IP address.

![image](https://user-images.githubusercontent.com/110126036/186627173-3763c17f-43a9-4a63-a890-5d34d5e2cd58.png)


## Make an ASG for APP Group

![image](https://user-images.githubusercontent.com/110126036/186890683-642d3f82-8fe5-4aed-a633-325bbf239644.png)

### Make sure that App is fully functional and Save as an AMI
- Create as before
- Make a service which will start the app

Create a provisioning script to run the app.

```bash
#!/bin/bash

cd /home/ubuntu/cloud_computing/app

npm start
```

Create a service which will run the script.

```
[Unit]

Description=Run the app
After=default.target

[Service]

ExecStart=/bin/bash /home/ubuntu/cloud_computing/serviceapp.sh

[Install]

WantedBy=default.target

```
- Enable and Restart the service using sudo systemctl.

### Create a Launch Template with User data

User data:
```
#!/bin/bash

sudo systemctl start nodeapp.service

sudo systemctl enable nodeapp.service

sudo systemctl restart nodeapp.service
```

### Create AutoScaling Group

![image](https://user-images.githubusercontent.com/110126036/186872834-8522878e-49e2-4652-aa5c-62865687a19d.png)
![image](https://user-images.githubusercontent.com/110126036/186872937-3d16125a-ea69-4b48-82e1-9f3ff26bccc9.png)
![image](https://user-images.githubusercontent.com/110126036/186873130-a24be00e-8c8e-4160-9d7b-de4360fc74a7.png)
![image](https://user-images.githubusercontent.com/110126036/186873367-13f9e711-3198-4b41-97a4-5fac6ead7370.png)
![image](https://user-images.githubusercontent.com/110126036/186873462-432e1be2-5ccc-4528-ad7d-77ef08e40866.png)
![image](https://user-images.githubusercontent.com/110126036/186873601-a5b6141d-3ce3-401a-aba2-a6662dee9442.png)


