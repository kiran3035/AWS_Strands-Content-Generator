# 🚀 Quick Start - 5 Minutes to Your First Generated Content

This is the fastest way to get started with the Strands Agent Content Generator.

## Prerequisites Check

Before you begin, make sure you have:
- [ ] Python 3.8+ installed (`python --version`)
- [ ] AWS Account created
- [ ] AWS Bedrock access enabled (see SETUP_GUIDE.md if not)
- [ ] AWS credentials ready (Access Key ID & Secret Key)

## 5-Minute Setup

### 1. Install Dependencies (1 minute)

Open terminal in this directory and run:

```bash
pip install -r requirements.txt
```

### 2. Start Jupyter (30 seconds)

```bash
jupyter notebook
```

Your browser will open automatically.

### 3. Open the Notebook (10 seconds)

Click on: `strands_content_generator.ipynb`

### 4. Configure AWS Credentials (1 minute)

**Option 1 (Recommended)**: Use AWS CLI:
```bash
aws configure
```

**Option 2**: Set environment variables:
```bash
# Windows PowerShell
$env:AWS_ACCESS_KEY_ID="your-access-key"
$env:AWS_SECRET_ACCESS_KEY="your-secret-key"
$env:AWS_DEFAULT_REGION="us-east-1"
```

The notebook automatically loads credentials from your environment or `~/.aws/credentials` file.

### 5. Run Everything (2 minutes)

Click: **Cell** → **Run All**

Wait for all cells to execute. You'll see:
- ✅ Outline generation
- ✅ Quality evaluation
- ✅ Content development
- ✅ Final polished document

### 6. Check Your Output (30 seconds)

Look in the `generated_content/` folder for your markdown file!

---

## What You Just Did

You created a complete technical guide using 5 AI agents:
1. **OutlineGenerator** - Created the structure
2. **OutlineEvaluator** - Checked quality
3. **CoherenceEnhancer** - Improved flow
4. **ContentDeveloper** - Wrote detailed sections
5. **QualityAssurance** - Final polish

---

## Next Steps

### Customize the Topic

Change this line in the notebook:
```python
TOPIC = "Your Custom Topic Here"
```

### Try Different Models

```python
# Faster & cheaper
model_id="anthropic.claude-3-haiku-20240307-v1:0"

# More capable (default)
model_id="anthropic.claude-3-sonnet-20240229-v1:0"
```

### Adjust Quality Threshold

```python
# More strict (requires higher quality)
if evaluation_results.score < 0.9:  # was 0.8
```

---

## Troubleshooting

### "No module named 'boto3'"
```bash
pip install boto3
```

### "NoCredentialsError"
- Check you copied credentials correctly
- No extra spaces or quotes

### "ModelNotFoundError"
- Enable Claude 3 in AWS Bedrock console
- See SETUP_GUIDE.md for detailed steps

### "AccessDeniedException"
- Verify IAM permissions
- See SETUP_GUIDE.md Section 3

---

## Cost Estimate

**Per notebook run**: $0.10 - $0.50

This generates approximately:
- 1 comprehensive outline
- 5-7 detailed sections
- 2,000-4,000 words of polished content

---

## Files in This Project

```
strands-content-generator/
├── strands_content_generator.ipynb  ← Main notebook (START HERE)
├── README.md                         ← Full documentation
├── SETUP_GUIDE.md                    ← Detailed AWS setup
├── QUICK_START.md                    ← This file
├── requirements.txt                  ← Python dependencies
├── .env.template                     ← Config template
├── .gitignore                        ← Git ignore rules
└── generated_content/                ← Output folder (created automatically)
```

---

## Need More Help?

- **Complete Setup**: Read `SETUP_GUIDE.md`
- **Full Documentation**: Read `README.md`
- **AWS Issues**: Check AWS Console → Bedrock → Model access

---

**That's it! You're ready to generate amazing content with AI! 🎉**
