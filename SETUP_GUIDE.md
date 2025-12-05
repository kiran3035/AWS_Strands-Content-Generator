# Complete Setup Guide - Strands Agent Content Generator

This guide will walk you through setting up and running the Strands Agent Content Generator from scratch.

## Table of Contents
1. [AWS Account Setup](#aws-account-setup)
2. [Enable AWS Bedrock](#enable-aws-bedrock)
3. [Create IAM User & Credentials](#create-iam-user--credentials)
4. [Local Environment Setup](#local-environment-setup)
5. [Running the Notebook](#running-the-notebook)
6. [Troubleshooting](#troubleshooting)

---

## 1. AWS Account Setup

### Create an AWS Account (if you don't have one)

1. Go to [aws.amazon.com](https://aws.amazon.com)
2. Click **Create an AWS Account**
3. Follow the registration process
4. Add a payment method (required, but we'll stay in free tier where possible)
5. Verify your identity
6. Choose the **Basic Support Plan** (free)

**Cost Warning**: AWS Bedrock is NOT part of the free tier. Expected costs: $0.10-$0.50 per notebook run.

---

## 2. Enable AWS Bedrock

### Step-by-Step:

1. **Sign in to AWS Console**: [console.aws.amazon.com](https://console.aws.amazon.com)

2. **Navigate to Bedrock**:
   - In the search bar, type "Bedrock"
   - Click on **Amazon Bedrock**

3. **Check Region**:
   - Ensure you're in a supported region (top-right corner)
   - Recommended: **us-east-1** (N. Virginia) or **us-west-2** (Oregon)

4. **Request Model Access**:
   - In the left sidebar, click **Model access**
   - Click **Modify model access** (orange button)
   - Find **Anthropic** section
   - Check the box for **Claude 3 Sonnet**
   - Optionally check **Claude 3 Haiku** (cheaper alternative)
   - Click **Request model access**

5. **Wait for Approval**:
   - Usually instant for Claude models
   - Status will change from "In progress" to "Access granted"
   - Refresh the page after 30 seconds

---

## 3. Create IAM User & Credentials

### Why IAM User?
For security, we'll create a dedicated user with minimal permissions instead of using root credentials.

### Step-by-Step:

1. **Navigate to IAM**:
   - Search for "IAM" in AWS Console
   - Click **Identity and Access Management**

2. **Create User**:
   - Click **Users** in left sidebar
   - Click **Create user**
   - User name: `bedrock-content-generator`
   - Click **Next**

3. **Set Permissions**:
   - Select **Attach policies directly**
   - Click **Create policy** (opens new tab)
   
4. **Create Custom Policy**:
   - In the new tab, click **JSON** tab
   - Paste this policy:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "bedrock:InvokeModel",
           "bedrock:InvokeModelWithResponseStream"
         ],
         "Resource": [
           "arn:aws:bedrock:*::foundation-model/anthropic.claude-3-sonnet-*",
           "arn:aws:bedrock:*::foundation-model/anthropic.claude-3-haiku-*"
         ]
       }
     ]
   }
   ```
   - Click **Next**
   - Policy name: `BedrockInvokeOnly`
   - Click **Create policy**

5. **Attach Policy to User**:
   - Go back to the user creation tab
   - Click refresh button next to policy search
   - Search for `BedrockInvokeOnly`
   - Check the box
   - Click **Next**
   - Click **Create user**

6. **Create Access Keys**:
   - Click on the newly created user
   - Click **Security credentials** tab
   - Scroll to **Access keys**
   - Click **Create access key**
   - Select **Application running outside AWS**
   - Click **Next**
   - Description: `Content Generator Notebook`
   - Click **Create access key**

7. **Save Credentials** ⚠️ IMPORTANT:
   - **Access Key ID**: Copy this (looks like `AKIAIOSFODNN7EXAMPLE`)
   - **Secret Access Key**: Copy this (looks like `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`)
   - Click **Download .csv file** for backup
   - **You won't be able to see the secret key again!**
   - Click **Done**

---

## 4. Local Environment Setup

### Prerequisites
- Python 3.8 or higher
- pip (Python package manager)
- Git (optional)

### Step-by-Step:

1. **Open Terminal/Command Prompt**:
   - Windows: Press `Win + R`, type `cmd`, press Enter
   - Mac/Linux: Open Terminal app

2. **Navigate to Project Directory**:
   ```bash
   cd C:\Users\kiran\.gemini\antigravity\scratch\strands-content-generator
   ```

3. **Create Virtual Environment** (recommended):
   ```bash
   python -m venv venv
   ```

4. **Activate Virtual Environment**:
   - Windows:
     ```bash
     venv\Scripts\activate
     ```
   - Mac/Linux:
     ```bash
     source venv/bin/activate
     ```

5. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

6. **Verify Installation**:
   ```bash
   python -c "import boto3; print('boto3 version:', boto3.__version__)"
   ```

---

## 5. Running the Notebook

### Method 1: Jupyter Notebook (Recommended for Beginners)

1. **Start Jupyter**:
   ```bash
   jupyter notebook
   ```

2. **Browser Opens Automatically**:
   - If not, copy the URL from terminal (looks like `http://localhost:8888/...`)
   - Paste in your browser

3. **Open the Notebook**:
   - Click on `strands_content_generator.ipynb`

4. **Configure Credentials**:
   - **Option 1 (Recommended)**: Use AWS CLI:
     ```bash
     aws configure
     ```
     Enter your Access Key ID, Secret Access Key, and region when prompted.
   
   - **Option 2**: Set environment variables:
     ```bash
     # Windows PowerShell
     $env:AWS_ACCESS_KEY_ID="your-access-key"
     $env:AWS_SECRET_ACCESS_KEY="your-secret-key"
     $env:AWS_DEFAULT_REGION="us-east-1"
     
     # Linux/Mac
     export AWS_ACCESS_KEY_ID="your-access-key"
     export AWS_SECRET_ACCESS_KEY="your-secret-key"
     export AWS_DEFAULT_REGION="us-east-1"
     ```
   
   The notebook automatically loads credentials from environment variables or `~/.aws/credentials` file.

5. **Run the Notebook**:
   - Click **Cell** → **Run All**
   - Or press `Shift + Enter` to run cells one by one

### Method 2: JupyterLab (Advanced Interface)

1. **Start JupyterLab**:
   ```bash
   jupyter lab
   ```

2. **Follow same steps as Method 1**

---

## 6. Troubleshooting

### Issue: "ModuleNotFoundError: No module named 'boto3'"

**Solution**:
```bash
pip install boto3
```

### Issue: "NoCredentialsError"

**Solution**:
- Double-check you copied the credentials correctly
- Ensure no extra spaces or quotes
- Verify the IAM user has the correct policy attached

### Issue: "ModelNotFoundError" or "ResourceNotFoundException"

**Solution**:
- Verify you enabled Claude 3 models in Bedrock console
- Check you're in the correct AWS region
- Wait a few minutes if you just requested access

### Issue: "AccessDeniedException"

**Solution**:
- Verify IAM policy is attached to user
- Check the policy JSON is correct
- Ensure you're using the correct access keys

### Issue: "ThrottlingException"

**Solution**:
- You're making too many requests too quickly
- Wait a few seconds between requests
- Implement retry logic (shown in notebook)

### Issue: Jupyter won't start

**Solution**:
```bash
# Reinstall Jupyter
pip install --upgrade jupyter notebook

# Or use JupyterLab
pip install jupyterlab
jupyter lab
```

### Issue: High AWS Costs

**Solution**:
- Check CloudWatch for usage metrics
- Set up billing alerts in AWS Console
- Use Claude 3 Haiku instead of Sonnet (cheaper)
- Reduce `max_tokens` parameter

---

## Cost Monitoring

### Set Up Billing Alerts:

1. Go to AWS Console → **Billing Dashboard**
2. Click **Budgets** in left sidebar
3. Click **Create budget**
4. Select **Cost budget**
5. Set amount: `$10` (or your preference)
6. Add email notification
7. Click **Create budget**

### Monitor Usage:

1. Go to **Cost Explorer**
2. Filter by service: **Bedrock**
3. View daily costs
4. Expected: $0.10-$0.50 per notebook run

---

## Next Steps

Once everything is working:

1. ✅ Run the notebook completely
2. ✅ Review the generated content in `./generated_content/`
3. ✅ Experiment with different topics
4. ✅ Modify agent prompts
5. ✅ Try different Claude models
6. ✅ Deploy to production (see README.md)

---

## Security Reminders

⚠️ **NEVER commit credentials to Git**
⚠️ **Don't share your access keys**
⚠️ **Rotate keys regularly**
⚠️ **Use IAM roles in production**
⚠️ **Enable MFA on your AWS account**

---

## Getting Help

- **AWS Support**: [AWS Support Center](https://console.aws.amazon.com/support/)
- **Bedrock Documentation**: [AWS Bedrock Docs](https://docs.aws.amazon.com/bedrock/)
- **Boto3 Documentation**: [Boto3 Docs](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)

---

**You're all set! 🚀 Happy generating!**
