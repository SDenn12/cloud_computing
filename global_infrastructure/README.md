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
