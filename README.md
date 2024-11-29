# Fetching Secrets from AWS Secrets Manager using Python

This guide demonstrates how to store, retrieve, and use secrets stored in AWS Secrets Manager using AWS CLI and Python.

## Step 1: Create a Secret in AWS Secrets Manager

1. **Go to the AWS Secrets Manager Console:**
   - Log in to your AWS account and navigate to [Secrets Manager](https://console.aws.amazon.com/secretsmanager/).

2. **Store a New Secret:**
   - Click on `Store a new secret`.
   - Select `Other type of secret`.
   - Under `Key/value pairs`, enter:
     - Key: `username`, Value: `user`
     - Key: `password`, Value: `password`
   - Click `Next`.

3. **Name the Secret:**
   - Enter a name like `mysecretkeys`.

4. **Save the Secret:**
   - Review your settings and click `Store secret`.

## Step 2: Retrieve the Secret Using AWS CLI

1. **Install and Configure AWS CLI:**
   - Install AWS CLI if you haven't already. For installation instructions, check [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).
   - Configure the CLI with your credentials:
     ```bash
     aws configure
     ```
     Provide your AWS Access Key, Secret Key, and default region.

2. **Retrieve the Secret:**
   - Run the following command to retrieve the secret:
     ```bash
     aws secretsmanager get-secret-value --secret-id mysecretkeys --output json
     ```

## Step 3: Use the Secret in a Python Script

1. **Install Boto3:**
   - Install the AWS SDK for Python:
     ```bash
     pip install boto3
     ```

2. **Create a Python Script:**
   - Save the following code as `secret.py`:

     ```python
     import boto3
     import json

     # Initialize the Secrets Manager client
     client = boto3.client('secretsmanager', region_name='ap-south-1')  # Replace 'your-region'

     secret_name = "mysecretkeys"

     try:
         # Fetch the secret value
         response = client.get_secret_value(SecretId=secret_name)
         secret = json.loads(response['SecretString'])

         print(f"Username: {secret['username']}")
         print(f"Password: {secret['password']}")
     except Exception as e:
         print(f"Error fetching secret: {e}")
     ```

3. **Run the Script:**
   - Execute the script in your terminal:
     ```bash
     python secret.py
     ```

   - You should see the following output:
     ```
     Username: user
     Password: password
     ```

## Conclusion

This process demonstrates how to securely store and retrieve sensitive data using AWS Secrets Manager and how to access these secrets using Python. Make sure to follow security best practices when handling secrets and sensitive data in your applications.
