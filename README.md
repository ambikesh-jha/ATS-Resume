# ATS Resume Scorer

## Overview
The ATS Resume Scorer is an AI-powered tool that evaluates resumes against a given job description. It uses the **Groq LLM** to extract key information, compare resume content with job requirements, and assign an **ATS score** based on relevance.

## Features
- Extracts text from **PDF resumes**
- Compares resumes against job descriptions
- Assigns an **ATS score (0-100)** based on skill match, experience, qualifications, and keyword relevance
- Identifies **missing keywords**
- Provides a **detailed profile summary**
- Returns results in **JSON format**

## Installation
Ensure you have the required dependencies installed:

```bash
pip install PyPDF2 groq
```

## Usage
### 1. Set Up API Key
Ensure you have a **Groq API key** stored securely:

```python
from google.colab import userdata
sec_key = userdata.get("GROQ_API_KEY")
```

### 2. Initialize ATS Scorer
```python
ats_scorer = ATSScorer(api_key=sec_key)
```

### 3. Process Resumes from a Folder
```python
resume_folder = "/content/Resumes"  # Path to folder containing PDFs
resume_texts = ats_scorer.process_resume_folder(resume_folder)
```

### 4. Define Job Description
```python
job_description = """
# AI and ML Engineer

## Responsibilities
- Design and develop AI and ML models using Python
- Implement Deep Learning and NLP techniques

## Required Qualifications
- 3 years of experience in AI and ML
- Bachelor's or Master's degree in Computer Science
- Proficiency in:
  - Python
  - AI & Machine Learning
  - Deep Learning
  - NLP
  - Computer Vision
  - Generative AI
"""
```

### 5. Score Resumes
```python
results = {}
for filename, resume_text in resume_texts.items():
    if resume_text:
        result = ats_scorer.calculate_ats_score(resume_text, job_description)
        results[filename] = result
    else:
        results[filename] = ats_scorer._error_response(f"Failed to extract text from {filename}")
```

### 6. Output Results
```python
import json
print(json.dumps(results, indent=2))
```

## JSON Output Format
The output JSON contains detailed information about the candidate and their ATS score:

```json
{
  "Candidate Name": "John Doe",
  "Mail ID": "johndoe@example.com",
  "Phone Number": "+1234567890",
  "Address": "123 Main St, City, Country",
  "Job Role": "AI Engineer",
  "Nationality": "American",
  "Contract Type": "Full-Time",
  "Education": [
    {"degree": "MSc in Computer Science", "institution": "XYZ University", "duration": "2018-2020"}
  ],
  "Work Experience": [
    {"company": "TechCorp", "position": "AI Engineer", "duration": "3 years", "keyResponsibilities": ["Developed AI models", "Implemented NLP techniques"]}
  ],
  "ATS Score": 85,
  "Missing Keywords": ["Generative AI"],
  "Profile Summary": {
    "Summary": "Candidate meets most requirements but lacks experience with Generative AI.",
    "Scoring Breakdown": {
      "Skills": 40,
      "Experience": 25,
      "Qualifications": 20,
      "Keywords": 8,
      "Deductions": 8
    }
  }
}
```

## Notes
- The ATS score is computed based on **skills, experience, qualifications, and keywords**
- The tool **automatically extracts** key details from resumes
- Results are **validated for correctness** before returning

scratch code link : https://colab.research.google.com/drive/1HDJ8Bq-1Cwp6K7PGT-yyhGNT5gkuxA0M?usp=sharing

