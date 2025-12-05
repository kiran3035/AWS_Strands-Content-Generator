# Strands Agent Content Generator - AWS Bedrock Implementation

A complete, production-ready multi-agent content generation system using AWS Bedrock and the Strands Agent framework.

## 🎯 Use Case

This project demonstrates a **Technical Documentation Generator** that uses multiple specialized AI agents to create comprehensive, high-quality technical guides on AWS services. The system implements a 7-step pipeline from outline generation to final polished content.

## 📋 What This Notebook Teaches You

1. **AWS Bedrock Integration** - How to configure and use AWS Bedrock with Claude 3 models
2. **Multi-Agent Architecture** - Building specialized agents with distinct roles and responsibilities
3. **Quality Evaluation** - Implementing automated evaluation and iterative improvement loops
4. **Content Pipeline** - Creating a complete content generation workflow
5. **Production Deployment** - Best practices for deploying to AWS infrastructure

## 🏗️ Architecture

The system uses 6 specialized agents:

```
┌─────────────────────────────────────────────────────────────┐
│                    Content Generation Pipeline               │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. OutlineGenerator    → Creates comprehensive outline     │
│  2. OutlineEvaluator    → Assesses quality & completeness   │
│  3. CoherenceEnhancer   → Improves flow & transitions       │
│  4. ContentDeveloper    → Writes detailed sections          │
│  5. StyleRefiner        → Polishes tone & readability       │
│  6. QualityAssurance    → Final error checking              │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 Quick Start

### Prerequisites

- Python 3.8+
- AWS Account with Bedrock access
- AWS credentials (Access Key ID & Secret Access Key)
- Jupyter Notebook or JupyterLab

### Installation

1. **Clone or download this project**

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
   
   Or manually:
   ```bash
   pip install boto3 python-dotenv jupyter
   ```

3. **Enable AWS Bedrock Models**:
   - Go to AWS Console → Bedrock → Model access
   - Request access to **Claude 3 Sonnet** model
   - Wait for approval (usually instant)

4. **Configure AWS Credentials**:
   - **Option 1 (Recommended)**: Use AWS CLI:
     ```bash
     aws configure
     ```
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
   - The notebook automatically loads credentials from environment or `~/.aws/credentials`

5. **Run the notebook**:
   ```bash
   jupyter notebook strands_content_generator.ipynb
   ```

## 📖 Notebook Structure

The notebook is organized into 7 main steps:

### Step 1: Initial Outline Generation
- Configures an agent to create comprehensive content outlines
- Uses specialized prompting for technical documentation
- Generates structured outlines with main sections and key points

### Step 2: Outline Evaluation
- Implements quality assessment criteria
- Scores outlines on completeness, structure, depth, and practicality
- Automatically requests improvements if quality threshold not met

### Step 3: Coherence Enhancement
- Analyzes logical flow between sections
- Adds transition statements
- Ensures progressive complexity

### Step 4: Section Development
- Generates detailed content for each section
- Maintains context across sections
- Includes code examples and practical guidance

### Step 5: Content Refinement
- Applies style and tone improvements
- Ensures consistent terminology
- Optimizes readability for target audience

### Step 6: Quality Assurance
- Performs comprehensive error checking
- Validates technical accuracy
- Ensures professional quality output

### Step 7: Final Output & Export
- Exports polished content to markdown file
- Adds metadata and formatting
- Provides analytics on the generation process

## 💰 Cost Considerations

AWS Bedrock pricing (as of 2024):
- **Claude 3 Sonnet**: ~$3 per 1M input tokens, ~$15 per 1M output tokens
- **Estimated cost per run**: $0.10 - $0.50 (depending on content length)

### Cost Optimization Tips:
- Use Claude 3 Haiku for simpler tasks (cheaper)
- Set appropriate `max_tokens` limits
- Implement caching for repeated queries
- Monitor usage with CloudWatch

## 🔒 Security Best Practices

**For Production Deployment:**

1. **Never hardcode credentials** - Use IAM roles or AWS Secrets Manager
2. **Use least-privilege IAM policies**:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "bedrock:InvokeModel"
         ],
         "Resource": "arn:aws:bedrock:*::foundation-model/anthropic.claude-3-sonnet-*"
       }
     ]
   }
   ```
3. **Enable CloudTrail logging** for audit trails
4. **Use VPC endpoints** for private connectivity
5. **Encrypt data** at rest and in transit

## 📊 Monitoring & Observability

The notebook includes built-in analytics:
- Agent interaction counts
- Token usage tracking
- Content length metrics
- Performance timing

**For Production:**
- Integrate with CloudWatch for metrics
- Use X-Ray for distributed tracing
- Set up alarms for error rates
- Monitor costs with AWS Cost Explorer

## 🎨 Customization

### Change the Topic
```python
TOPIC = "Your Custom Topic Here"
```

### Modify Agent Behavior
Edit the `system_prompt` for any agent:
```python
outline_agent = BedrockAgent(
    name="OutlineGenerator",
    system_prompt="Your custom instructions here..."
)
```

### Adjust Quality Thresholds
```python
if evaluation_results.score < 0.9:  # Increase from 0.8
    # Request improvements
```

### Use Different Models
```python
agent = BedrockAgent(
    name="MyAgent",
    model_id="anthropic.claude-3-haiku-20240307-v1:0"  # Cheaper, faster
)
```

## 🚢 Production Deployment

### Option 1: AWS Lambda
```python
# Deploy as serverless function
# Use Lambda layers for dependencies
# Trigger via API Gateway or EventBridge
```

### Option 2: ECS/Fargate
```dockerfile
# Containerize with Docker
FROM python:3.9-slim
COPY . /app
RUN pip install -r requirements.txt
CMD ["python", "content_generator.py"]
```

### Option 3: Step Functions
```yaml
# Orchestrate multi-agent workflow
# Handle retries and error handling
# Parallel processing for multiple documents
```

## 📁 Output

Generated content is saved to:
```
./generated_content/aws_lambda_guide_YYYYMMDD_HHMMSS.md
```

Each file includes:
- Metadata header (title, author, date)
- Complete formatted content
- Code examples and best practices
- Professional markdown formatting

## 🐛 Troubleshooting

### "ModelNotFoundError"
- Ensure you've enabled Claude 3 models in Bedrock console
- Check your AWS region supports Bedrock

### "AccessDeniedException"
- Verify your AWS credentials are correct
- Check IAM permissions include `bedrock:InvokeModel`

### "ThrottlingException"
- Implement retry logic with exponential backoff
- Request quota increase in AWS Service Quotas

### "ValidationException"
- Check `max_tokens` is within model limits
- Ensure request body format is correct

## 📚 Additional Resources

- [AWS Bedrock Documentation](https://docs.aws.amazon.com/bedrock/)
- [Claude 3 Model Card](https://www.anthropic.com/claude)
- [Strands AI Framework](https://github.com/strands-ai)
- [Boto3 Documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)

## 🤝 Contributing

This is a learning project. Feel free to:
- Modify for your use cases
- Add new agent types
- Implement additional evaluation criteria
- Extend with new features

## 📄 License

This project is provided as-is for educational purposes.

## ⚠️ Important Notes

1. **AWS Costs**: Monitor your usage to avoid unexpected charges
2. **Rate Limits**: Bedrock has default quotas (requests per minute)
3. **Model Access**: Some regions may not have all models available
4. **Data Privacy**: Don't send sensitive data to LLM APIs without proper controls

---

## 🎓 Learning Path

**Beginner**: Run the notebook as-is to understand the workflow

**Intermediate**: Customize prompts and add new evaluation criteria

**Advanced**: Deploy to production with monitoring and auto-scaling

---

**Happy Learning! 🚀**

*Built with AWS Bedrock, Claude 3, and the Strands Agent Framework*
