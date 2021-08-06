<a style="float: right;" href="https://githubsfdeploy.herokuapp.com?owner=mattandneil&amp;repo=aws-sdk&amp;ref=master">
    <img src="https://www.xbaf.com/wp-content/uploads/gh-aws-deploy.png" />
</a>

# Amazon Web Services SDK for Salesforce Apex

The AWS SDK for Salesforce makes it easy for developers to access Amazon Web Services in their Apex code, and build robust applications and software using services like Amazon S3, Amazon EC2, etc.

Get started by installing the package: <a href="https://login.salesforce.com/packaging/installPackage.apexp?p0=04t6g000008SbHP"><b>/packaging/installPackage.apexp?p0=04t6g000008SbHP</b></a>

###### Sign up then go to your AWS Console > Security Credentials > Access Keys:

<img src="https://www.xbaf.com/wp-content/uploads/gh-aws-security-setup.png" />

#### Create a Named Credential

- **Name**: S3
- **URL**: https://s3.us-east-1.amazonaws.com/
- **Identity Type**: Named Principal
- **Auth Protocol**: AWS Signature V4
- **AWS Access Key ID**: [your key here]
- **AWS Secret Access Key**: [your secret here]
- **AWS Region**: us-east-1
- **AWS Service**: s3

## Amazon Simple Notification Service (SNS) SDK

SNS is an infrastructure for delivering messages. Publishers communicate asynchronously with subscribers by producing and sending a message to a topic. Subscribers include web servers / email addresses / Amazon SQS queues / AWS Lambda functions.

<img src="https://www.xbaf.com/wp-content/uploads/gh-aws-sns-how-works.png" />

###### Create a topic:

    AWS.SNS.CreateTopicRequest request = new AWS.SNS.CreateTopicRequest();
    request.url = 'https://sns.us-east-1.amazonaws.com/';
    request.name = 'Your_Topic';
    AWS.SNS.CreateTopicResponse response = new AWS.SNS.CreateTopic().call(request);
    String topic = response.createTopicResult.topicArn;

###### Publish messages:

    AWS.SNS.PublishRequest request = new AWS.SNS.PublishRequest();
    requset.url = 'https://sns.us-east-1.amazonaws.com/';
    request.message = 'Test_Message';
    request.topicArn = 'arn:aws:sns:us-east-1:123456789012:Your_Topic';
    AWS.SNS.PublishResponse response = new AWS.SNS.Publish().call(request);

## Amazon Simple Storage Service (S3) SDK

The [Apex client](https://github.com/mattandneil/aws-sdk/blob/master/S3.cls) manipulates both buckets and contents. You can create and destroy objects, given the bucket name and the object key.

<img src="https://www.xbaf.com/wp-content/uploads/gh-aws-flow-signup.png" />

###### Create a bucket:

    AWS.S3.CreateBucketRequest request = new AWS.S3.CreateBucketRequest();
    request.url = 'https://s3.us-east-1.amazonaws.com/bucket';
    AWS.S3.CreateBucketResponse response = new AWS.S3.CreateBucket().call(request);

###### Adding an object to a bucket:

    AWS.S3.PutObjectRequest request = new AWS.S3.PutObjectRequest();
    request.url = 'https://s3.us-east-1.amazonaws.com/bucket/key.txt';
    request.body = Blob.valueOf('test_body');
    AWS.S3.PutObjectResponse response = new AWS.S3.PutObject().call(request);

###### View an object:

    AWS.S3.GetObjectRequest request = new AWS.S3.GetObjectRequest();
    requset.url = 'https://s3.us-east-1.amazonaws.com/bucket/key.txt';
    AWS.S3.GetObjectResponse response = new AWS.S3.GetObject().call(request);
    System.debug(response.body); // 'test_body'

###### List bucket contents:

    AWS.S3.ListObjectsRequest request = new AWS.S3.ListObjectsRequest();
    requset.url = 'https://s3.us-east-1.amazonaws.com/bucket';
    AWS.S3.ListObjectsResponse response = new AWS.S3.ListObjects().call(request);

###### Delete an object:

    AWS.S3.DeleteObjectRequest request = new AWS.S3.DeleteObjectRequest();
    request.url = 'https://s3.amazonaws.com/bucket/key.txt';
    AWS.S3.DeleteObjectResponse response = new AWS.S3.DeleteObject().call(request);

## Amazon Elastic Cloud Compute (EC2) SDK

EC2 provides scalable computing capacity in the cloud. The [Apex client](https://github.com/mattandneil/aws-sdk/blob/master/EC2.cls) calls services to launch instances, terminate instances, etc. The API responds synchronously, but bear in mind that the the instance state transitions take time.

<img src="https://www.xbaf.com/wp-content/uploads/gh-aws-instance-lifecycle.png" />

###### Describe running instances:

    AWS.EC2.DescribeInstancesRequest request = new AWS.EC2.DescribeInstancesRequest();
    request.url = 'https://ec2.amazonaws.com/';
    AWS.EC2.DescribeInstancesResponse response = new AWS.EC2.DescribeInstances().call(request);

###### Launch a new instance:

    AWS.EC2.RunInstancesRequest request = new AWS.EC2.RunInstancesRequest();
    request.url = 'https://ec2.amazonaws.com/';
    request.imageId = 'ami-08111162';
    AWS.EC2.RunInstancesResponse response = new AWS.EC2.RunInstances().call(request);

###### Terminate an existing instance:

    AWS.EC2.TerminateInstancesRequest request = new AWS.EC2.TerminateInstancesRequest();
    request.url = 'https://ec2.amazonaws.com/';
    request.instanceId = new List<String>{'i-01234567890abcdef'};
    request.dryRun = true;
    AWS.EC2.TerminateInstancesResponse response = new AWS.EC2.TerminateInstances().call(request);
