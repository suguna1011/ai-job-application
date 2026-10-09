# 🤖 AI Job Application Automation System

An AI-powered web application that helps simplify the job application process by analyzing job descriptions, matching resume skills, generating customized resumes, creating HR messages, and sending job applications through email.

## 📌 Project Overview

Applying for jobs manually can take a lot of time. This project automates several steps of the job application process using **FastAPI, Python, Docker, Nginx, and PostgreSQL**.

Users can analyze job descriptions, upload their resumes, identify matching skills, generate customized resume PDFs, create professional HR messages, preview their applications, and send emails with resume attachments.

## ✨ Features

- **Job Description Analysis:** Analyze job descriptions and identify required skills.
- **Resume Analysis:** Upload and analyze a candidate's resume.
- **Job and Resume Matching:** Compare job requirements with resume skills.
- **Customized Resume Generation:** Generate a customized resume in PDF format.
- **AI-Powered HR Message Generation:** Create professional application messages based on job and candidate details.
- **Application Preview:** Preview the recipient, email subject, message, and resume before sending.
- **Email Automation:** Send job applications to HR with the customized resume attached.
- **Resume Viewing:** Open and view the generated PDF resume.
- **Database Integration:** Use PostgreSQL to store and retrieve application data.
- **Docker Deployment:** Run the application services using Docker Compose.
- **Nginx Reverse Proxy:** Route incoming web requests to the FastAPI application.

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Python | Backend programming |
| FastAPI | Backend API framework |
| PostgreSQL | Database |
| Docker | Application containerization |
| Docker Compose | Manage multiple containers |
| Nginx | Reverse proxy and web entry point |
| Uvicorn | Run the FastAPI application |
| SMTP | Send application emails |
| ReportLab | Generate PDF documents |
| AWS EC2 | Cloud hosting and deployment |
| HTML, CSS, JavaScript | Web interface |

## 🏗️ Project Architecture

```text
                  User / Browser
                        |
                        v
                  Nginx :80
                        |
                        v
                 FastAPI :8000
                  /     |      \
                 /      |       \
                v       v        v
          PostgreSQL  Resume   Email
             :5432    PDF      SMTP
                     Generator
```

### How It Works

1. The user opens the web application through the browser.
2. Nginx receives the request on port 80 and forwards it to FastAPI.
3. FastAPI processes the job description and resume information.
4. The application matches the job requirements with the candidate's skills.
5. The system generates a customized resume PDF and an HR message.
6. The user previews the application details.
7. FastAPI sends the application email with the resume attached.
8. PostgreSQL supports the application's data storage and retrieval.

## 📂 Project Structure

```text
ai-job-application/
│
├── app/
│   ├── main.py
│   ├── config.py
│   │
│   ├── database/
│   │   └── database.py
│   │
│   └── services/
│       ├── email_sender.py
│       ├── jd_parser.py
│       ├── job_database.py
│       ├── job_matcher.py
│       ├── job_source.py
│       ├── message_generator.py
│       ├── resume_generator.py
│       └── resume_parser.py
│
├── nginx/
│   └── nginx.conf
│
├── resumes/
├── generated_resumes/
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md
```

## ⚙️ Prerequisites

Before running the project, make sure you have:

- An AWS EC2 Ubuntu server or another Linux machine.
- Docker installed.
- Docker Compose V2 installed.
- Git installed.
- An SMTP-enabled email account and appropriate credentials.
- Any required AI API key configured in the environment, if your selected application features use one.

## 🚀 Installation and Setup

### Step 1: Clone the Repository

```bash
git clone https://github.com/suguna1011/ai-job-application.git
```

Move into the project directory:

```bash
cd ai-job-application
```

### Step 2: Configure Environment Variables

Create your environment file:

```bash
cp .env.example .env
```

Open it:

```bash
nano .env
```

Configure the environment variables required by your application, including SMTP and any required AI API credentials.

Example SMTP configuration:

```env
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USERNAME=your-email@gmail.com
SMTP_PASSWORD=your-google-app-password
SENDER_EMAIL=your-email@gmail.com
```

Replace the example values with your own credentials. For Gmail, use an App Password where required, not your normal account password.

**Security note:** Never upload your `.env` file, passwords, API keys, or other secrets to GitHub.

### Step 3: Build the Docker Images

```bash
docker compose build
```

This command builds the application image using the Dockerfile.

### Step 4: Start the Application

```bash
docker compose up -d
```

Docker Compose starts the application, Nginx, and PostgreSQL services.

### Step 5: Check Container Status

```bash
docker compose ps
```

To check the application logs:

```bash
docker compose logs -f app
```

Press `Ctrl + C` to stop following the logs. This does not stop the containers.

### Step 6: Access the Web Application

Open your EC2 instance's public IP address in your browser:

```text
http://YOUR_EC2_PUBLIC_IP
```

Make sure your EC2 security group allows inbound HTTP traffic on port 80.

To view the generated resume, use:

```text
http://YOUR_EC2_PUBLIC_IP/view-resume
```

The resume must have been generated and saved at the expected path before you can view it.

## 🖥️ How to Use the Application

1. Open the web application.
2. Enter or paste the job description.
3. Click **Analyze Job**.
4. Upload your resume and analyze it.
5. Click **Match Job and Resume** to view the matching results.
6. Enter your candidate and company details.
7. Generate the customized resume.
8. Generate the HR message.
9. Enter the HR email address and review the subject and message.
10. Click **Preview Application** to check the details.
11. Confirm the information and click **Send Application**.

The generated resume PDF is stored in the `generated_resumes/` directory.

## 🐳 Docker Services

| Service | Purpose | Port |
|---|---|---|
| Nginx | Public web entry point and reverse proxy | 80 |
| FastAPI | Backend application | 8000 |
| PostgreSQL | Database | 5432 |

In the current Docker Compose configuration, only Nginx publishes a host port:

```yaml
ports:
  - "80:80"
```

FastAPI and PostgreSQL are accessible internally through the Docker network. This avoids exposing the application backend and database directly to the public internet.

## 📄 Important Files

- `app/main.py` — Defines the FastAPI application and API endpoints.
- `app/services/job_matcher.py` — Handles job and resume skill matching.
- `app/services/resume_generator.py` — Generates customized resume PDFs.
- `app/services/message_generator.py` — Generates HR application messages.
- `app/services/email_sender.py` — Sends application emails and attachments.
- `Dockerfile` — Defines how the application image is built.
- `docker-compose.yml` — Configures and runs the application services.
- `nginx/nginx.conf` — Configures Nginx to forward requests to FastAPI.
- `.env.example` — Provides an example environment configuration.

## 🔧 Useful Docker Commands

Check running containers:

```bash
docker compose ps
```

View application logs:

```bash
docker compose logs -f app
```

Restart services:

```bash
docker compose restart
```

Rebuild and restart the application after code changes:

```bash
docker compose up -d --build app
```

Stop services:

```bash
docker compose down
```

The named PostgreSQL volume is retained when using `docker compose down`. Avoid `docker compose down -v` unless you intentionally want to remove the database volume.

## 🔐 Security Considerations

- Do not commit `.env` or sensitive credentials to GitHub.
- Use an SMTP App Password or another appropriate credential method.
- Keep PostgreSQL port 5432 private.
- Keep the FastAPI port 8000 private when using Nginx as the public entry point.
- Restrict SSH access to trusted IP addresses.
- Use HTTPS
