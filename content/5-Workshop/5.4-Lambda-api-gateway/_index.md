---
title: "Lambda + API Gateway"
weight: 4
chapter: false
pre : " <b> 5.4. </b> "
---

Overview

This lab demonstrates how to build and deploy Lambda-backed HTTP APIs using Amazon API Gateway (HTTP API). Students will create a simple Lambda function, connect it to an HTTP API route, test end-to-end, and add observability (CloudWatch). The lab also covers permissions (IAM role), CORS, and basic error handling.

Prerequisites

- AWS account with permission to create Lambda, API Gateway, IAM roles, CloudWatch.
- AWS CLI or Console access.
- Node.js (if running local function tests) or use inline code from the lab.

Learning objectives

- Create and deploy an AWS Lambda (Node.js) function.
- Create an API Gateway HTTP API and integrate with Lambda.
- Configure route, CORS, and test HTTP methods.
- Set up IAM role/permissions for Lambda execution and API invocation.
- Observe function execution via CloudWatch logs and metrics.

Lab steps (high level)

1. Create a Lambda function
   - Use the Console or AWS CLI. Choose Node.js runtime and a minimal handler that returns JSON.
   - Create or attach an execution role with `AWSLambdaBasicExecutionRole` (CloudWatch logs) and other needed permissions.
2. Test the Lambda locally (optional)
   - Use `sam local invoke` or `node` to sanity-check the handler.
3. Create an API Gateway (HTTP API)
   - Create a new HTTP API and add an integration pointing to the Lambda function.
   - Add routes (e.g., `GET /status`, `POST /items`).
4. Configure CORS
   - Enable CORS for routes that will be called from the browser and set allowed origins/headers/methods.
5. Deploy and test
   - Deploy the API and call the endpoints via `curl` or a browser (for GET) and `fetch`/Postman for POST.
   - Verify responses and HTTP status codes.
6. Observability
   - Inspect CloudWatch Logs for the Lambda function to view logs and errors.
   - Check CloudWatch Metrics (invocations, errors, duration) to understand performance.
7. Error handling and retries
   - Return meaningful HTTP status codes from Lambda and handle client errors vs server errors.
8. Cleanup
   - Delete the API, Lambda function, and IAM role (if created specifically for the lab).

Sample minimal Lambda (Node.js)

```js
exports.handler = async (event) => {
  return {
    statusCode: 200,
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ message: 'OK', input: event })
  };
};
```

Testing examples

- curl GET:

  curl -i https://<api-id>.execute-api.<region>.amazonaws.com/status

- curl POST:

  curl -i -X POST https://<api-id>.execute-api.<region>.amazonaws.com/items -H "Content-Type: application/json" -d '{"name":"test"}'

Troubleshooting

- If you see 502/500, check CloudWatch Logs for the Lambda for stack traces.
- Verify the Lambda execution role has correct permissions for CloudWatch Logs and any other AWS services used.
- Check API CORS settings for browser calls.

References

- AWS Lambda Developer Guide
- Amazon API Gateway (HTTP API) Developer Guide

