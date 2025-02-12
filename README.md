# AWS ElasticBeanstalk (EB)
Learner's AWS ElasticBeanstalk CLI steps to version update; this is a repo containing a sample python codebase for learning how to update the version package bundle on AWS EB.

### Step 1: Clone the Repository (If Not Already Cloned)
````
git clone https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
````

#### Replace YOUR_REPOSITORY with your Elastic Beanstalk environment name.

### Step 2: Install the Elastic Beanstalk CLI (If Not Installed)
Ensure you have the AWS Elastic Beanstalk CLI installed:
````
pip install awsebcli --upgrade
````

### Step 3: Initialize Elastic Beanstalk (If Not Initialized)
````
eb init
````
- Select your AWS region.
- Choose the Elastic Beanstalk application associated with your project.
- Select the Python platform (e.g., Python 3.8 running on 64bit Amazon Linux 2).
- Set up the SSH key pair (optional but recommended).

### Step 4: Create the New Version Package Bundle
#### 1. Create a ZIP archive of the application code:
````
zip -r app-v0.0.2.zip . -x "*.git*" "*__pycache__*" "*.DS_Store*"
````
This will package the necessary files excluding Git metadata and cache files.

#### 2. Upload the ZIP file to an S3 bucket (Replace with your bucket name):

````
aws s3 cp app-v0.0.2.zip s3://YOUR_S3_BUCKET/
````

### Step 5: Create a New Application Version in Elastic Beanstalk

````
aws elasticbeanstalk create-application-version \
  --application-name YOUR_APPLICATION_NAME \
  --version-label v0.0.2 \
  --source-bundle S3Bucket="YOUR_S3_BUCKET",S3Key="app-v0.0.2.zip"
````

Replace YOUR_APPLICATION_NAME with your Elastic Beanstalk application name.
Replace YOUR_S3_BUCKET with your S3 bucket name.

### Step 6: Deploy the New Version to Your Environment

````
aws elasticbeanstalk update-environment \
  --environment-name YOUR_ENVIRONMENT_NAME \
  --version-label v0.0.2
````

Replace YOUR_ENVIRONMENT_NAME with your Elastic Beanstalk environment name.

### Step 7: Verify Deployment
Check the environment status:
````
eb status
````

#### View logs for debugging:
````
eb logs
````

### Optional: Roll Back to a Previous Version
If the new version has issues, you can roll back to v0.0.1:
````
aws elasticbeanstalk update-environment \
  --environment-name YOUR_ENVIRONMENT_NAME \
  --version-label v0.0.1
````

****
