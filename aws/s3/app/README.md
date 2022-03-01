# AWS SDK S3 App

Example S3 app built off of AWS SDK for Go. This app also utilizes [minikube](https://minikube.sigs.k8s.io/docs/start/).

## Setup
### IAM Credentials
[IAM credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) are needed to access AWS services along with the region .  Both the **Access Key ID** and **Secret Access Key** are needed. If using credentials from awscli, both keys can be found at `~/.aws/credentials`.  Both these keys need to be converted to base64 and inserted in the `secret.yaml` under `access_key` and `secret_key` respectively.  


### Bucket
An [AWS Bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/GetStartedWithS3.html) will need to be setup beforehand. Please refer to the AWS S3 Documentation on how to [Create An S3 Bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html). If using a pre-existing bucket, ensure that your AWS account has [proper access and roles to the bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-access-control.html).

From the bucket you will need the **Bucket name** along with an **Object Key**. The [Bucket's Region](https://docs.aws.amazon.com/AmazonS3/latest/API/API_control_Region.html) is also needed. In `secret.yaml`, add the S3 Bucket's region to the `region` data.
## Build
1. Start minikube
2. Navigate to

   `~/sbo-services-investigations/app`

3. Run the command:
   
   `eval $(minikube docker-env)`

4. Build the Docker image using the command:

   `docker build -t aws-sdk-test .`

5. Run the commands:
   
   `kubectl apply -f secret.yaml `
   
   `kubectl apply -f app.yaml`
 
    
## Run App

Once the app has been built, run the command with the namespace the app is installed:

`kubectl get pods -n <namespace>`

Get the name of the pod and run the command, replacing `aws-sdk-pod` with the name of the pod.

`kubectl exec --stdin <aws-sdk-pod> -n <namespace>  -- sh`

Finally, run the command with the name of the bucket being used and the key you want for the test file:

`go run aws-sdk-test.go -b <bucket> -k <key> -d 10m`

If the application runs successfully, a new test file will be uploaded to the defined bucket and key


