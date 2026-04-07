# Phase 1: AWS Bedrock & RAG Setup

## 1. Workflow Overview

**S3 Bucket** (Data Source) $\rightarrow$ **Knowledge Base (KB)** (Vector Store) $\rightarrow$ **Bedrock Agent** (Orchestration)

## 2. Implementation Steps

### **Step 1: Data Storage**

- **Action:** Configured **Amazon S3** and uploaded source documents.

### **Step 2: Vectorization**

- **Action:** Set up **Knowledge Base (KB)** and linked it to the S3 bucket.
- **Verification:** Ran sync and test queries directly in the **KB Console**.

### **Step 3: Intelligence Integration**

- **Action:** Connected KB to **Amazon Bedrock Agent** using the **Amazon Nova** model.
- **Verification:** Performed end-to-end testing in the **Agent Console**.

## 3. Developer Notes

- **Tech Stack:** AWS Bedrock (Model: Amazon Nova).
- **UI Tip:** The Bedrock Agent instruction box is small; draft system prompts in an external editor (VS Code/Notion) for better readability.

---

# Phase 2: Lambda Function Integration

## 1. Workflow Overview

**Bedrock Agent** $\leftrightarrow$ **Lambda Function** (Business Logic) $\rightarrow$ **Console Testing**

## 2. Implementation Steps

### **Step 1: Function Creation**

- **Action:** Created a standalone **AWS Lambda** function to handle Bedrock Agent requests.
- **Pivot:** Simplified the scope to a direct **Bedrock-to-Lambda** connection for initial validation to avoid `ResourceNotFound` errors from API Gateway.

### **Step 2: Configuration & Optimization**

- **Action:** Adjusted **General Configuration** settings.
- **Setting:** Increased **Timeout** from 3s to **30s – 1min**.
- **Reasoning:** LLM processing takes longer than the default 3s; this prevents premature function termination.

### **Step 3: Verification**

- **Action:** Conducted functional testing directly within the **Lambda Console** "Test" tab.

## 3. Developer Notes

- **Insight:** Decouple components during development. Ensure Lambda-Bedrock logic works before adding API layers.
- **IAM:** Always verify that the Execution Role has permissions to invoke Bedrock models.

---

# Phase 3: API Gateway Integration & CLI Testing

## 1. Workflow Overview

**CLI (curl)** $\rightarrow$ **API Gateway** $\rightarrow$ **Lambda Function** $\rightarrow$ **Bedrock**

## 2. Implementation Steps

### **Step 1: Configuration & Optimization After Errors**

- **Action:** Established the **ANY** route to handle catch-all HTTP methods.
- **Integration:** Attached **Lambda Proxy Integration** to the **ANY** route.
- **Authorization:** Reapplied security protocols to the new route to match the original POST method.
- **Sync Logic:** Synchronized Route, Integration, and Auth to prevent `501` (Not Implemented) or `403` (Forbidden) errors.

### **Step 2: End-to-End Verification**

- **Action:** Executed final validation via the **CLI** using **curl** statements.
- **Result:** Confirmed the API Gateway successfully triggers the backend and returns the Bedrock response.

## 3. Developer Notes

- **Methodology:** Using the `ANY` method simplifies the handling of **CORS preflight** (OPTIONS) and data requests (POST) in a single configuration.

---

# Phase 4: Frontend Integration & Browser Testing

## 1. Workflow Overview

**Browser (Localhost)** $\rightarrow$ **API Gateway** $\rightarrow$ **Lambda** $\rightarrow$ **Bedrock**

## 2. Implementation Steps

### **Step 1: Local Development Environment**

- **Initial Attempt:** Opening **index.html** directly (`file://`) failed due to **CORS** restrictions.
- **Action:** Hosted the frontend on a local development server at **http://localhost:8000**.
- **Reasoning:** Browsers require a valid HTTP origin to satisfy CORS policies. Local hosting provides the necessary headers that raw files lack.

### **Step 2: Browser Verification**

- **Action:** Conducted end-to-end testing directly within the **Browser**.
- **Result:** Verified real-time communication between the UI and the Bedrock backend.

## 3. Developer Notes

- **CORS Tip:** If browser requests fail while `curl` works, the issue is almost certainly the **CORS** configuration in API Gateway or an invalid Origin.

### ⚠️ Critical Troubleshooting: Regional Consistency

During the deployment of AWS resources, Region Mismatch is a primary cause of integration failure.

The Issue: AWS services are region-scoped. If your Amazon Bedrock (Nova model) is active in us-east-2 (Ohio), but your Lambda or API Gateway defaults to us-east-1 (N. Virginia), they will be unable to "see" each other.

The Symptom: You will encounter ResourceNotFoundException or AccessDenied even if your IAM roles and code are perfectly correct.

#### The Fix:

1.  Always verify the region code in the top-right corner of the AWS Console for every service.
2.  In your Lambda code (using Boto3), explicitly set the region:
    boto3.client('bedrock-agent-runtime', region_name='us-east-2').
3.  Ensure your API Gateway endpoint and the Lambda it triggers reside in the same geographic region to minimize latency and avoid cross-region transfer costs.

# 🚀 Global Troubleshooting Manual: From Browser to AWS Backend

### Step 1: Browser Console (Front-end Logic)
* **Objective:** Confirm if the issue is a JavaScript crash or a failure to load static resources.
* **Path:** `F12` > **Console** tab.
* **Key Errors:** * `Uncaught ReferenceError` (Variable not defined).
    * `404 Not Found` (Missing assets/scripts).

### Step 2: Network Panel (Front-end & Back-end Interface)
* **Objective:** Check if the request was sent successfully and inspect the initial status returned by the backend.
* **Path:** `F12` > **Network** tab > Click on the specific **Request Name**.

#### 1. Headers (Identity & Environment)
* **Status Code:** `401/403` (Auth/Token issues), `502/504` (Gateway/Timeout).
* **Request Headers:** Verify if the `Authorization` field is present and correct.
* **Response Headers:** Locate **`x-amzn-RequestId`**. This is your unique key for locating logs within AWS.

#### 2. Response Body (Direct Backend Feedback)
* **Path:** **Network** > **Response** sub-tab.
* **Key Errors:** Look for messages like `{"message": "Invalid API Key"}`. If detailed info is provided here, the fix usually doesn't require diving into AWS.

### Step 3: AWS Backend Logs (Deep Logic Investigation)
* **Objective:** When the Network panel shows a `500` error with no details, use logs to find specific code execution errors.
* **Workflow (AWS Console):**
    * **Navigation Path:** `Lambda` > `Functions` > `BedrockAgentConnector`
    * **Selected Tab:** `Monitor`
    * **Action:** Click **View CloudWatch logs**.
    * **Filter Suggestion:** Set to **Local timezone** to easily match the log entry with the exact time the browser error occurred.
* **Core Action:** Enter the latest **Log Stream**, and copy the **most recent (top-most)** logs for AI analysis.

