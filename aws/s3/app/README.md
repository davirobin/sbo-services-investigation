# AWS SDK S3 App

Example S3 app built off of AWS SDK for Go.

## Setup
For the setup, [IAM credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) are needed to access AWS services along with the region .  Both the Access Key ID and Secret Access Key are needed. If using credentials from awscli, both keys can be found at `~/.aws/credentials`.  Both these keys need to be converted to base64 and replaced in the `secret.yaml` under `access_key` and `secret_key` respectively.  

For the region, choose the [AWS Region](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html)  the service is on. In `secret.yaml`, replace `region` with the base64 region of the service.

A bucket will also be needed to connect to.

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


