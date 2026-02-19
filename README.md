# n8n Flows Collection

This repository contains n8n workflow automations for various business processes. The flows are designed to run locally and can be deployed to Railway for cloud hosting.

## 📁 Available Flows

### 1. Hiring Agent
- **Description**: Automated hiring workflow that monitors Google Drive for new resume uploads and processes them through AI analysis
- **Trigger**: Google Drive folder monitoring (Resume Uploader For AI Agent)
- **Key Features**:
  - Automatic resume download when new files are added
  - AI-powered resume processing
  - Integration with Google Drive

### 2. TheBatraNumerology Email Automation
- **Description**: Email automation system for numerology report generation and delivery
- **Trigger**: Email-based workflow
- **Key Features**:
  - Google Sheets integration for report data
  - Automated email processing
  - Numerology report generation
  - Dynamic content filtering based on email IDs

## 🚀 Getting Started

### Prerequisites
- Node.js (v16 or higher)
- Docker (optional, for containerized deployment)
- Google Cloud credentials (for Google Drive/Sheets integration)
- Railway account (for cloud deployment)

### Local Development Setup

#### Option 1: Using n8n Desktop App
1. Download and install the [n8n desktop app](https://n8n.io/download/)
2. Launch n8n on your local machine
3. Import the flow files:
   - Click "Import from file"
   - Select the `.json` flow files from this repository
   - Configure credentials for each service

#### Option 2: Using n8n CLI
```bash
# Install n8n globally
npm install n8n -g

# Start n8n locally
n8n start

# Access n8n at http://localhost:5678
```

#### Option 3: Using Docker
```bash
# Pull the n8n Docker image
docker pull n8nio/n8n

# Run n8n container
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

### Importing Flows

1. Open n8n interface (http://localhost:5678)
2. Click "Import from file" in the workflows section
3. Select the `.json` files:
   - `Hiring Agent.json`
   - `TheBatraNumerology Email Automation for Report Sheet.json`
4. Configure the required credentials for each service

## 🔧 Configuration

### Required Credentials

#### For Hiring Agent Flow:
- **Google Drive OAuth2 API**: Access to Google Drive for resume monitoring
- **AI Service Credentials**: For resume processing (configure based on your AI provider)

#### For Numerology Email Automation:
- **Google Sheets OAuth2 API**: Access to numerology report data
- **Email Service Credentials**: SMTP or email provider credentials

### Setting Up Credentials in n8n

1. Go to **Settings** → **Credentials**
2. Click **Add credential**
3. Select the appropriate service (Google Drive, Google Sheets, etc.)
4. Follow the OAuth2 setup process
5. Test the connection and save

## ☁️ Railway Deployment

### Step 1: Prepare for Deployment

1. Create a `railway.toml` file in your project root:
```toml
[build]
builder = "NIXPACKS"

[deploy]
startCommand = "n8n start"
healthcheckPath = "/healthz"
healthcheckTimeout = 100
restartPolicyType = "ON_FAILURE"
restartPolicyMaxRetries = 10

[env]
N8N_BASIC_AUTH_ACTIVE = "true"
N8N_BASIC_AUTH_USER = "admin"
N8N_BASIC_AUTH_PASSWORD = "your_secure_password"
WEBHOOK_URL = "https://your-app-name.railway.app/"
```

### Step 2: Deploy to Railway

1. **Create Railway Account**: Sign up at [railway.app](https://railway.app)

2. **Install Railway CLI**:
```bash
npm install -g @railway/cli
```

3. **Login to Railway**:
```bash
railway login
```

4. **Initialize Project**:
```bash
railway init
```

5. **Deploy**:
```bash
railway up
```

### Step 3: Configure Environment Variables

In your Railway project dashboard, add these environment variables:

```bash
# n8n Configuration
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=your_username
N8N_BASIC_AUTH_PASSWORD=your_secure_password
WEBHOOK_URL=https://your-app-name.railway.app/

# Google Credentials (replace with your actual values)
GOOGLE_DRIVE_CLIENT_ID=your_google_client_id
GOOGLE_DRIVE_CLIENT_SECRET=your_google_client_secret
GOOGLE_SHEETS_CLIENT_ID=your_google_client_id
GOOGLE_SHEETS_CLIENT_SECRET=your_google_client_secret

# Database (Railway provides PostgreSQL)
DATABASE_URL=postgresql://user:password@host:port/database
```

### Step 4: Import Flows to Railway Instance

1. Access your n8n instance at `https://your-app-name.railway.app`
2. Login with the basic auth credentials you set
3. Import the flow files as described in the local setup

## 🔗 Self-Hosted Network Integration

### Connecting to Your n8n Portal

Once deployed on Railway, you can integrate your flows with your self-hosted network:

1. **Update Webhook URLs**: Modify webhook URLs in your flows to point to your Railway instance
2. **Configure External Services**: Update any external service callbacks to use your Railway URL
3. **Set Up Custom Domain** (optional):
   - In Railway, go to Settings → Domains
   - Add your custom domain
   - Update DNS records as instructed

### Monitoring and Maintenance

- **Health Checks**: Railway automatically monitors your app health
- **Logs**: View logs in Railway dashboard under the "Logs" tab
- **Scaling**: Configure auto-scaling in Railway settings based on your needs

## 🛠️ Troubleshooting

### Common Issues

1. **Credential Errors**:
   - Ensure OAuth2 credentials are properly configured
   - Check that required scopes are granted

2. **Webhook Timeouts**:
   - Increase timeout settings in Railway configuration
   - Optimize flow execution time

3. **Memory Issues**:
   - Upgrade your Railway plan for more resources
   - Optimize large data processing in flows

### Debug Mode

Enable debug logging by adding this environment variable:
```bash
DEBUG=n8n:*
```

## 📚 Additional Resources

- [n8n Documentation](https://docs.n8n.io/)
- [Railway Documentation](https://docs.railway.app/)
- [Google OAuth2 Setup Guide](https://developers.google.com/identity/protocols/oauth2)

## 🤝 Contributing

1. Fork this repository
2. Create a new branch for your feature
3. Add or modify flows
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🆘 Support

If you encounter issues:
1. Check the troubleshooting section above
2. Review n8n and Railway documentation
3. Open an issue in this repository with details about your problem

---

**Note**: Always secure your credentials and never commit sensitive information to version control. Use environment variables for all sensitive data.
