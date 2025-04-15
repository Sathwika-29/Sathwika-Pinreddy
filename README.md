# Serverless_Task_Manager 📝



<img width="1190" alt="Screenshot 2025-04-05 at 6 48 22 PM" src="https://github.com/user-attachments/assets/ddfa89d5-844c-4d2b-a892-94288b26f58b" />






A simple serverless ToDo API built using **AWS Lambda**, **API Gateway**, and **DynamoDB**. This project demonstrates how to build a RESTful backend using AWS services without managing any servers.

---

## 🧰 Tech Stack

- AWS Lambda (Python)
- Amazon DynamoDB
- Amazon API Gateway

---

## 🚀 Features

- ✅ Add a new task
- 📋 Retrieve all tasks
- 🔁 Fully serverless (no servers to manage)
- 🧪 Easy to test with curl/Postman

---

## 📋 Steps to Build the Project

### 🔹 Step 1: Create DynamoDB Table

1. Go to **DynamoDB** in the AWS Console.
2. Click **Create Table**
   - Table name: `ToDoTable`
   - Partition key: `taskId` (String)
3. Leave everything else as default and click **Create**

---

### 🔹 Step 2: Create Lambda Functions

#### 🔸 1. AddTask Function

Go to **AWS Lambda** → Click **Create Function**  
Function Name: `AddTask`  
Runtime: `Python 3.x`  
Permissions: Create or use existing role with **AmazonDynamoDBFullAccess**

**Lambda Code:**

```python
import boto3
import json
import uuid

def lambda_handler(event, context):
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table('ToDoTable')

    task = json.loads(event['body'])
    task['taskId'] = str(uuid.uuid4())

    table.put_item(Item=task)

    return {
        'statusCode': 200,
        'body': json.dumps({'message': 'Task added successfully', 'task': task})
    }

json code

{
  "body": "{\"taskName\": \"Learn AWS\", \"status\": \"Pending\"}"
}

Function Name: GetTasks

import boto3
import json

def lambda_handler(event, context):
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table('ToDoTable')
    response = table.scan()
    return {
        'statusCode': 200,
        'body': json.dumps(response['Items'])
    }

json

{}

ADD a Task

curl -X POST https://<your-api-id>.execute-api.<region>.amazonaws.com/stage-1/tasks \
-H "Content-Type: application/json" \
-d '{"taskName": "Learn AWS", "status": "Pending"}'

Get All Tasks

curl -X GET https://<your-api-id>.execute-api.<region>.amazonaws.com/stage-1/tasks

