
# AWS Bedrock Lambda Function Example

This project demonstrates how to set up and invoke an AWS Bedrock model using an AWS Lambda function. You'll learn how to create a Lambda function, configure necessary permissions, and run a test to receive a response from the AWS Bedrock service.

## Prerequisites

- **AWS Account** with access to AWS Bedrock models.
- **AWS Lambda** and **IAM** permissions to create and manage Lambda functions and policies.
- **Basic Python knowledge** (we use Python 3.12 in this example).

## Steps to Set Up

### 1. Get Access to AWS Bedrock Models

- Request access to the AWS Bedrock foundation models by navigating to **AWS Bedrock** in the AWS Management Console.
- Go to **Bedrock configurations** and select **model access**.

### 2. Create the Lambda Function

1. In the AWS Management Console, go to **AWS Lambda** and create a new function.
2. Name the function (for example): `AWSBEDROCK_DEMO`.
3. Choose **Python 3.12** as the runtime.
4. Create the function.

### 3. Add Permissions to Invoke AWS Bedrock Models

1. In the Lambda function console, navigate to the **Configuration** tab.
2. Under **Permissions**, click on the role name (this will open in a new tab).
3. Click on **Add permissions** and select **Create inline policy**.
4. In the policy editor:
   - Select **Service**: `bedrock`.
   - Choose **Actions**: `Invoke-model`.
   - Under **Resources**, select **All resources**.
5. Click **Next** and give the policy a name, such as `BedrockInvokeAccess`.
6. Click **Create policy**.

### 4. Add Code to the Lambda Function

1. Go back to the **Code** section of the Lambda function.
2. Replace the default code with the following Python script:

    ```python
    import boto3
    import json

    def lambda_handler(event, context):
        bedrock = boto3.client(
            service_name='bedrock-runtime', 
            region_name='us-east-1'
        )
        
        input = {
            "modelId": "cohere.command-text-v14",
            "contentType": "application/json",
            "accept": "*/*",
            "body": "{\"prompt\":\"What is AWS Bedrock?\",\"max_tokens\":400,\"temperature\":0.75,\"p\":0.01,\"k\":0,\"stop_sequences\":[],\"return_likelihoods\":\"NONE\"}"
        }

        response = bedrock.invoke_model(
            body=input["body"],
            modelId=input["modelId"],
            accept=input["accept"],
            contentType=input["contentType"]
        )

        response_body = json.loads(response['body'].read())
        print(response_body)
    ```

### 5. Deploy the Lambda Function

After adding the code, click **Deploy** to save and deploy the function.

### 6. Create a Test Event

1. Click the **Test** button at the top of the Lambda function console.
2. Create a new test event:
   - **Event name**: Provide any name (e.g., `TestBedrockEvent`).
3. Save the test event.

### 7. Run the Test

1. Click **Test** to invoke the Lambda function.
2. If the test times out, follow the next step.

### 8. Adjust Timeout Settings

1. In the Lambda function, go to **Configuration**.
2. Under **General configuration**, increase the timeout value to **15 minutes** (to handle any potential delays in model invocation).

### 9. View the Response

- Once the Lambda function runs successfully, it will return a response.
- The desired output will be under a tag called **"Text"** within the response body.

---

## Conclusion

This project demonstrates how easy it is to invoke AWS Bedrock models from a Lambda function. AWS Bedrock offers a scalable, serverless way to use large language models for various applications.

---

## Troubleshooting

- **Timeout Errors**: If you experience timeouts, make sure you've increased the Lambda function's timeout to 15 minutes.
- **Permissions Issues**: Double-check that your Lambda function's IAM role has the correct inline policy with `Invoke-model` permissions for AWS Bedrock.

---

Enjoy working with AWS Bedrock! 🚀

