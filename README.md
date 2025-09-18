# Serverless Contact Form Backend

A **serverless backend project** built using AWS services to handle contact form submissions in a scalable and cost-effective way.

---

## 🚀 Features
- **AWS Lambda** for backend logic  
- **Amazon API Gateway** to expose a REST API endpoint  
- **Amazon DynamoDB** to store submitted form data  
- **Python** for writing the Lambda function  

---

## ⚙️ How it Works
1. A POST request is sent to the API Gateway endpoint.  
2. API Gateway triggers the Lambda function.  
3. The Lambda function:  
   - Parses the request data (`name`, `email`, `message`)  
   - Stores it in **DynamoDB**  
   - Returns a success response  

---

## 🛠️ Tech Stack
- **AWS Lambda** – serverless compute for backend code  
- **Amazon API Gateway** – REST API endpoint  
- **Amazon DynamoDB** – NoSQL database for submissions  
- **Python** – Lambda code  

---

## 📌 Example Test
Example POST request:

```bash
curl -X POST https://<api-gateway-id>.execute-api.<region>.amazonaws.com/prod/contact \
   -H "Content-Type: application/json" \
   -d '{"name":"Kalyani", "email":"kalyani@example.com", "message":"Hello from AWS!"}'

                                Response

{
  "statusCode": 200,
  "body": "Form submission stored successfully!"
}

                             
## 💻 Lambda Function Code

```python
# import json
import boto3
from datetime import datetime

# Initialize DynamoDB
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('ContactFormSubmissions')  # replace with your DynamoDB table name

def lambda_handler(event, context):
    try:
        # Parse the body from event
        body = json.loads(event['body'])

        name = body['name']
        email = body['email']
        message = body['message']

        # Put item into DynamoDB
        response = table.put_item(
            Item={
                'id': datetime.utcnow().isoformat(),
                'name': name,
                'email': email,
                'message': message
            }
        )

        return {
            'statusCode': 200,
            'body': json.dumps('Form submission stored successfully!')
        }

    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps(f"Error: {str(e)}")
        }





