## Amazon Transcribe & Email Integration
Amazon Transcribe makes it easy for developers to add speech to text capabilities (also known as ASR) to their applications. Audio data is virtually impossible for computers to search and analyze. Therefore, recorded speech needs to be converted to text before it can be used in applications. This PoC shows how can an audio file be converted to text using a simple Email workflow. A user can record an audio on their smart phone and send it to an email inbox as an attachment. Some back-end service monitoring that Inbox gets an "email-received" notification and sends the audio file to the Transcribe service for the transcription. Once the ASR process completes, the response text file is then emailed as an attachment to the sender.

## Architecture

See the high-level Architecture [Architecture](ArchitectureDiagram.svg).

## Tech Stack (Services/Frameworks)

- Amazon Transcribe
- Amazon WorkMail
- AWS Lambda (Python)
- AWS Lambda Layers (Java)
- Amazon DynamoDB
- Amazon S3
- Amazon SES
- AWS SDK

## Setup

1. Install AWS CLI, Gradle (optionally Maven) and Java8
2. Clone the repo 
3. Create an user in WorkMail assign an email address like user@abc.awsapps.com
4. Create a Lambda function (from the code EmailProcessorLambda.py) with Python 3.8 Runtime
5. Modify S3BucketName and DynamoRegion properties in the function accordingly
6. Assign S3, DynamoDB and WorkMail permissions to the role used by this lambda function
7. Create a rule by going to WorkMail console -> Organization Settings -> Inbound Rules and setting Action to Run Lambda and specifying name of Lambda function created in earlier step and specify domain/email address for the filtering
8. In the project directory update BUCKET_NAME variable inside 1-create-bucket.sh script
9. Update Constants.java file accordingly 
10. Update template.yml file (s3 bucket name etc.)
11. Execute "create-bucket.sh"
12. Execute "build-layer.sh"
13. Execute "deploy.sh"
14. Confirm if Java Lambda function has been created and S3 event trigger has been configured correctly
15. Assign S3, DynamoDB, SES and Transcribe permissions to the role used by this lambda function
16. Update Timeout setting of this function to 5 minutes
17. Go to SES console and validate email addresses that are going to be used for testing (this is a MUST if the SES account is a Sandbox account) 
18. Send an email to the WorkMail inbox with an audio file attachment (WAV/M4A/MP3/MP4) and in the response email - the sender will receive the transcribed file 

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

