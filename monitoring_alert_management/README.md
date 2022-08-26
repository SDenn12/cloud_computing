## Monitoring and Alert Management

### What is Monitoring?

Monitoring is a method of reviewing, observing and managing the operational workflow in a cloud-based IT infrastructure. They are used to confirm the availability of the website and can implicitly be used to scale applications to current demand (through the use of SQS).

Therefore the main benefits are:

- Scalability 
- Availability

which in turn can save money (with 100% uptime) and create a better consumer experience.

### What are the 4 Golden Signals of Monitoring

#### Latency 

The time it takes to service a request. It is important to distinguish between latency of successful and failed requests as this could skew data.

#### Traffic

A measure of how much demand is being placed on your system. Measurement is usually http requests per second.

#### Errors

The rate of requests that fail, either http 500s or http200s that coupled the wrong content.

#### Saturation

'How full your service is'. A measure of your system fraction, emphasizing the resources which are most constrained.

### What is CloudWatch?

CloudWatch is a monitoring service for AWS cloud resources and the applications you run on Amazon Web Services. You can use Amazon CloudWatch to collect and track metrics, collect and monitor log files, set alarms, and automatically react to changes in your Amazon Web Services resources.

### What is SNS?

Simple Notification Service is a fully managed messaging service for both application to application and application to person communication. You can get the A2P nofitications via SMS, mobile push and email. 

### What is SQS?

Simple Queue Service allows you to decouple and scale microservices, distributed systems and serverless applications.

Benefits: 

- Eliminate administrative overhead
- Scale elastically
- Cost effective

### Setting up an Alarm in CloudWatch (for CPU Utilization)

![image](https://user-images.githubusercontent.com/110126036/186434905-afc03328-9e84-42a6-a287-f25c7370f95b.png)

![image](https://user-images.githubusercontent.com/110126036/186434979-d8ca0fe5-7a1a-472b-a5ce-7ce7a4c6205d.png)

![image](https://user-images.githubusercontent.com/110126036/186435070-63c51700-db79-40a2-9e5b-54872ccd4ad3.png)

