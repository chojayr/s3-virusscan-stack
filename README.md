# S3 VirusScan


## Features

* Uses ClamAV to scan "newly" added files on S3 buckets
* Updates ClamAV database every 3 hours automatically
* Publishes a message to SNS in case of a finding
* Can optionally delete compromised files automatically


## How does it work

1. Using the S3 event notification. the S3 bucket will send events to the SQS queue in case of newly created file(object)
2. A ruby script running as a daemon in the background on the EC2 instance will act as a worker that will pull the queue messages from SQS which the contents are the events from the S3 buckets of newly created objects.
3. The script will download the newly created object from the S3 bucket then perform clamscan on the downloaded object to verify if there's a threat on the file
4. Once a threat found on a scanned file it will delete the file and send an SNS notification to the subscribers
5. The script also perform an antivirus update every 3 hours


## To create a S3-VirusScan stack

1. Download or clone the template s3-virusscan-stack
2. Upload the salt-s3-virusscan.tar.gz to your prefer S3 bucket
2. Modify and provide the the correct parameter value on file "s3_virusscan_params.json" 
3. To deploy the stack run the following command below 
   $ aws cloudformation create-stack --stack-name <value> --template-body file://s3_virusscan_template.json --parameters file://s3_virusscan_params.json 

## Full Details 
For more details of the implementation kindly go [here](https://codebase.com) 
