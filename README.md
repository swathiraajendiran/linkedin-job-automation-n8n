# LinkedIn Job Application Automation (n8n)

This project automates the process of 
**finding LinkedIn job postings**, 
**evaluating skill match**, and 
**generating tailored resumes and cover letters** — all powered by [n8n](https://n8n.io/), OpenAI, and Google Sheets/Docs.

---

## 🚀 **FEATURES**
- **Job Discovery**: Fetch job posts via LinkedIn RSS feeds.
- **Filtering & Deduplication**: Clean unwanted jobs and avoid reprocessing duplicates.
- **Job Description Parsing**: Extract company details, requirements, skills, and ATS keywords using GPT.
- **Resume Caching & Structuring**: Store and reuse candidate’s resume in structured JSON.
- **Skill Match Evaluation**: Compare job requirements with candidate skills and return a rating (1–5).
- **Tailored Resume Generation**: Dynamically customize resume for each job posting.
- **Cover Letter Generation**: AI-generated, 100-word professional cover letter aligned with job description and company values.
- **Google Sheets Integration**: Store job posts, resumes, cover letters, and match ratings for easy access.

---

## 🛠️ **INSTALLATION (Self-hosted n8n)**

1. Install **n8n** globally with npm:
   ```bash
   npm install n8n -g
2. Start n8n:

       n8n
    n8n will run at: http://localhost:5678

---

## 📡 **LINKEDIN JOB FEED SETUP**

This project uses RSS.app to convert LinkedIn job search results into RSS feeds.

🔹 Step 1: Generate a LinkedIn Job Search URL

   Example (unsanitised):	
https://www.linkedin.com/jobs/search/currentJobId=4287372543&distance=25&f_E=3%2C4&f_SB2=43&f_TPR=a1755373037&geoId=103759532&keywords=data%20engineer&location=Chigwell%2C%20England%2C%20United%20Kingdom&origin=JOB_SEARCH_PAGE_JOB_FILTER&sortBy=R

🔹 Step 2: Sanitise the URL
	Remove unnecessary parameters:
 - currentJobId=.. → targets one job, not useful for feeds
 - origin=.. → UI state tracking, not needed
 - refresh=true → forces UI reload, irrelevant for automation
 - Replace geoId with a stable region ID (e.g., 102257491 = United Kingdom).

	✅ Sanitised Example:
	https://www.linkedin.com/jobs/search/?f_E=3%2C4&f_TPR=r86400&geoId=102257491&keywords=Data%20Engineer&spellCorrectionEnabled=true

🔹 Step 3: Generate RSS Feed
	Paste the sanitised URL into RSS.app to create a valid RSS feed URL. This RSS link is what you configure in the RSS Feed node in n8n.

---

## ⚙️ **WORKFLOW SETUP**

1. Import the workflow into your n8n instance:

	Go to Workflows > Import from File
	Select Linkedin-Job-Automation.json

2. Configure credentials:

  - OpenAI API → for GPT calls (expect to spend ~$5/month on API usage).
  - Google Sheets API → for storing job postings, resumes, and cover letters.
  - Google Docs API → to fetch your base resume.

4. Update the RSS Feed node with your sanitised LinkedIn RSS URL.
5. (Optional) Add a Cron Trigger to automate runs: 0 9,12,15 * * * ➝ Runs every day at 9 AM, 12 PM, and 3 PM.

---

## 📊 **WORKFLOW OVERVIEW**

1. Trigger → Manual or scheduled execution.
2. Fetch Jobs → Read LinkedIn RSS feed.
3. Filter Jobs → Keep only relevant roles (e.g., Engineer, London).
4. Deduplicate → Skip already processed job IDs.
5. Parse Job Description → Extract company metadata, skills, and ATS keywords.
6. Resume Handling → Cache and parse structured resume.
7. Skill Match → Compare candidate’s skills to job requirements (1–5 score).
8. Cover Letter → AI-generated tailored letter.
9. Tailored Resume → Resume customized for each job post.
10. Google Sheets Update → Save everything for review and applications.

---

## 📌 **EXAMPLE OUTPUTS**

- Skill Match Score: 4 (Good alignment with most skills).
- Tailored Resume: Resume adjusted with job-specific keywords.
- Cover Letter: A concise, professional 100-word letter.

---

