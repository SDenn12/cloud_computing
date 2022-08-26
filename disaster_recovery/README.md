## Disaster Recovery

![image](https://user-images.githubusercontent.com/110126036/186161316-209766dd-f597-4f8c-9241-eab357d0d092.png)

In the case of an unexpected disaster the program's data should be backed up.

Multiple methods:

- Multi AZs (azs euw-1a,1b,1c)
- Multi Region (Ireland, London)
- Multi Cloud Deployment (Azure/GCP)
- Hybrid Cloud (localhost & public cloud)

The more layers of backup you have the more the cost.

### S3 Buckets (simple Storage Service)

S3 Buckets are global and are therefore globally available. This means that if 1 AZ goes down you can download your backed-up files onto a different AZ.

S3 require dependencies such as python3.7, pip3, awscli and the configuration of aws access and secret keys.

AWS KEYS MUST NOT BE SHARED WITH ANYONE!

You can enter keys after entering `aws configure`

In order:
```
sudo apt-get install python -y
sudo apt install python3-pip
alias python=python3
sudo pip3 install awscli
aws configure
```
For the configuration use `eu-west-1` as region name and `json` for the output format
### Boto 3

The following creates an s3 bucket and then uploads a file from the instance to the bucket.

```python
import boto3

s3 = boto3.client('s3')

s3.create_bucket(Bucket='eng122-samuel-pybucket', CreateBucketConfiguration={'LocationConstraint':'eu-west-1'})

s3.upload_file('test.txt','eng122-samuel-pybucket','python/test.txt')
```

![image](https://user-images.githubusercontent.com/110126036/186197362-8c1306ac-9ecf-40be-8e5c-2185ce5ea8cd.png)


Now delete the test.txt file on the VM to test the download and delete capabilities using `sudo rm test.txt`

```python
import boto3

s3 = boto3.client('s3')

s3.download_file('eng122-samuel-pybucket', 'python/test.txt','downloaded_file.txt')
```

![image](https://user-images.githubusercontent.com/110126036/186199285-8a04a23a-210f-4a46-9656-a58ba09ad522.png)

to delete the file you can run

```python
import boto3

s3 = boto3.client('s3')

s3.delete_object(Bucket='eng122-samuel-pybucket', Key='python/test.txt')
```

![image](https://user-images.githubusercontent.com/110126036/186201079-61aba8ef-23e3-4849-8160-91c3357d34da.png)

to delete the bucket

```python
import boto3

s3 = boto3.client('s3')

s3.delete_bucket(Bucket='eng122-samuel-pybucket')
```