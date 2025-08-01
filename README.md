# Serverless File Sharing Platform

### Project Description:

The Serverless File Sharing Platform allows users to securely upload and download files via a simple HTTP API. It leverages AWS Lambda for serverless compute, API Gateway for RESTful API management, and Amazon S3 for durable and scalable object storage. Users can interact with the platform using any HTTP client (like Postman), making it versatile for various use cases that involve file sharing and storage.


Use Cases: 

* File Sharing: Users can upload files to the platform using a POST request, specifying the file name and content. This can be useful for sharing documents, images, or any other digital assets securely. 
* File Distribution: Content creators can distribute files (e.g., software updates, media files) to consumers by generating download links through the platform. 
* Collaborative Work: Teams can share project resources, documents, and data securely, facilitating collaboration across different locations.

### Project Architecture:

![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Architecture.png?raw=true)


### Steps to Build the Project:

## Prerequisites

1. AWS Account with appropriate permissions to create Lambda functions, API Gateway, and S3 buckets.
2. Passion to Learn!

## Steps to Deploy

* Step 1: Create an S3 bucket to store uploaded files:
* ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20121140.png?raw=true)

  ```bash
  my-file-sharing-bucket-aj
  ```

* Step 2: Create the Lambda function to handle file uploads (UploadFunction): \
  Name: MyUploadFunction \
  Runtime: Python 3.x \
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20122343.png?raw=true)
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20123008.png?raw=true)
  
  Execution role: IAM role with S3 write permissions \
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20122828.png?raw=true)
  Code: Use the UploadFunction Python code.
  

* Step 3: Create the Lambda function to handle file downloads (DownloadFunction): \
  Name: MyDownloadFunction \
  Runtime: Python 3.x \
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20123209.png?raw=true)
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20123225.png?raw=true)
  
  Execution role: IAM role with S3 write permissions \
  Code: Use the DownloadFunction Python code.

* Step 4: Create an API Gateway \
* ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20123404.png?raw=true)
  Name: my-file-sharing-api-1 \
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20123452.png?raw=true)
  
  Create two resources: /files with POST and GET methods. \
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20123613.png?raw=true)
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20123744.png?raw=true)
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20123856.png?raw=true)
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20124619.png?raw=true)
  For each method, configure Lambda integration with UploadFunction and DownloadFunction respectively. 
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20124820.png?raw=true)
  

* Step 5: Configure GET Method: \
  Method Request --> Edit --> Request validator --> Validate Query String Parameters and Headers \
  Method Request --> Edit --> Request Body --> text/plain \
  Integration Request --> Edit --> Mapping Templates --> Content Type: application/json --> Content Body: 
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20125023.png?raw=true)
  ```bash
  {
    "queryStringParameters": {
        "fileName": "$input.params('fileName')"
    }
  }
  ```
* Step 6: Configure POST Method: \
  Integration Request --> Edit --> Mapping Templates --> Content Type: text/plain --> Content Body: \
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20125210.png?raw=true)
  ```bash
  {
    "body" : "$input.body",
    "queryStringParameters" : {
        "fileName" : "$input.params('fileName')"
    }
  }
  ```
* Step 7: Deploy API Gateway: \
  Deploy the API to a stage (e.g., dev): \
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20125230.png?raw=true)
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20125436.png?raw=true)
  Click on "Actions" > "Deploy API". \
  Choose your stage (e.g., dev) and deploy. 

* Step 8: Testing \
    1. Upload a File \
       Use Postman or another HTTP client: \
       POST to https://<api-id>.execute-api.<region>.amazonaws.com/dev/files?fileName=test.txt \
       Set request body to raw and content type to text/plain. \
       Enter file content (e.g., Hello World!) and send the request.

       or

       Use CURL Utility:
       ```bash
       curl --location 'https://<api-id>.execute-api.<region>.amazonaws.com/dev/files?fileName=test.txt' \
       --header 'Content-Type: text/plain' \
       --data 'Hello Ajay, This is my first project!'
       ```

    3. Download a File \
       Use Postman or another HTTP client: \
       GET to https://<api-id>.execute-api.<region>.amazonaws.com/dev/files?fileName=test.txt \
       Verify file content is returned correctly.

       or

       Use CURL Utility
       ```bash
       curl --location 'https://<api-id>.execute-api.<region>.amazonaws.com/dev/files?fileName=test.txt'
       ```
    4. Result \
  ![Image](https://github.com/Ajaytipte/Serverless-File-Sharing-Platform-AWS-/blob/main/assests/Screenshot%202025-07-17%20131342.png?raw=true)
  

