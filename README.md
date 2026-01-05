# AWS Lambda Docker Deployment Pipeline

This project demonstrates a complete CI/CD pipeline deploying a Python Docker container to AWS Lambda.

## 🚀 Architecture

**Code** (`app/code.py`) -> **GitHub Actions** -> **AWS ECR** -> **AWS Lambda** -> **CloudWatch Logs**

## 📂 Project Structure

- `app/code.py`: The Python application logic (Lambda handler).
- `Dockerfile`: Defines the Lambda-compatible Docker image.
- `.github/workflows/main.yml`: CI/CD pipeline configuration.

## 🛠️ Setup & Deployment

### Prequisites
1.  **AWS Account**: With permissions for ECR and Lambda.
2.  **GitHub Secrets**: Configure the following secrets in your repo:
    - `AWS_ACCESS_KEY_ID`
    - `AWS_SECRET_ACCESS_KEY`

### Deployment Steps
1.  **Push Changes**: Simply push to the `main` branch.
    ```bash
    git push origin main
    ```
2.  **CI/CD Pipeline**: GitHub Actions will automatically:
    - Build the Docker image.
    - Login to Amazon ECR.
    - Push the image to your ECR repository (`2026/test-docker-app`).
3.  **Update Lambda**: 
    - Ensure your Lambda function is configured to use the container image from ECR.
    - If using the `latest` tag, you may need to manually trigger an image update in the Lambda console or use a deployment tool to update the function code.

## 🔍 Verification

1.  **Invoke Function**: Test the function via the AWS Lambda Console.
2.  **Check Logs**: View execution logs in **Amazon CloudWatch**.
    - Look for the output: `Printing Hello test-docker-app`

### API Gateway Integration

The Lambda function is configured to return a **Lambda Proxy Integration** response.
1.  **Create API**: Create an HTTP API or REST API in API Gateway.
2.  **Create Route**: Add a route (e.g., `GET /`) and integrate it with your Lambda function.
3.  **Test**: Invoke the API endpoint URL.
    - Expected Response: `{"message": "Hello test-docker-app from API Gateway!"}`

## 🐳 Local Testing

You can build and run the container locally to verify the build process, though local execution requires the Lambda Runtime Interface Emulator (RIE) to fully simulate the Lambda environment.

```bash
docker build -t lambda-test .
# Basic run (will just start the runtime interface, waiting for requests)
docker run -p 9000:8080 lambda-test
```
