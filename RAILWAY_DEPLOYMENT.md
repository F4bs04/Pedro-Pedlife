# Railway Deployment Guide for Pedro Agent

## Overview
This guide explains how to deploy the Pedro Agent project on Railway, a modern cloud platform that simplifies application deployment.

## Prerequisites
- Railway account (sign up at [railway.app](https://railway.app))
- Railway CLI installed (optional but recommended)
- Git repository with your code

## Deployment Methods

### Method 1: Deploy from GitHub (Recommended)

1. **Connect Repository**
   - Go to [railway.app](https://railway.app) and log in
   - Click "New Project"
   - Select "Deploy from GitHub repo"
   - Choose your repository

2. **Configure Environment Variables**
   - In your Railway project dashboard, go to "Variables"
   - Add the following environment variables:
     ```
     OPENAI_API_KEY=your_openai_api_key_here
     PUBMED_API_KEY=your_pubmed_api_key_here
     DATABASE_URL=sqlite:///data/enhanced_rag.db
     AGENT_NAME=Pedro
     AGENT_ROLE=Assistente Clínico Pediátrico
     RAG_DATABASE_PATH=data/enhanced_rag.db
     RAG_CHUNK_SIZE=1000
     RAG_OVERLAP=200
     PUBMED_MAX_RESULTS=5
     PUBMED_DELAY=1
     MAX_QUERY_LENGTH=1000
     RATE_LIMIT_REQUESTS=100
     RATE_LIMIT_WINDOW=3600
     DEBUG=false
     ```

3. **Deploy**
   - Railway will automatically detect your Python app
   - It will use the `railway.json` configuration for deployment settings
   - The build process will install dependencies from `requirements.txt`
   - Your app will start with the command specified in `railway.json`

### Method 2: Deploy with Railway CLI

1. **Install Railway CLI**
   ```bash
   npm install -g @railway/cli
   ```

2. **Login to Railway**
   ```bash
   railway login
   ```

3. **Initialize Project**
   ```bash
   railway init
   ```

4. **Set Environment Variables**
   ```bash
   railway variables set OPENAI_API_KEY=your_key_here
   railway variables set PUBMED_API_KEY=your_key_here
   # ... add other variables as needed
   ```

5. **Deploy**
   ```bash
   railway up
   ```

## Configuration Files

### railway.json
- Configures the build and deployment settings
- Specifies the start command and health check
- Sets restart policy

### Dockerfile (Alternative)
- If you prefer Docker-based deployment
- Railway will automatically detect and use the Dockerfile
- Provides more control over the build environment

### .env.railway.template
- Template for environment variables
- Copy to `.env` for local development
- Use Railway dashboard or CLI to set production variables

## Database Considerations

The current configuration uses SQLite with a local file (`data/enhanced_rag.db`). For production on Railway:

1. **SQLite (Current Setup)**
   - Works for small to medium applications
   - Data persists in Railway's ephemeral storage
   - Consider Railway's volume mounting for persistence

2. **PostgreSQL (Recommended for Production)**
   - Railway offers managed PostgreSQL
   - Better for concurrent users and data integrity
   - Update `DATABASE_URL` to PostgreSQL connection string

## Post-Deployment

1. **Check Logs**
   - Use Railway dashboard to monitor application logs
   - Check for any startup errors or missing environment variables

2. **Test Endpoints**
   - Railway provides a public URL for your application
   - Test your API endpoints to ensure everything works

3. **Monitor Performance**
   - Use Railway's built-in monitoring
   - Set up alerts for downtime or errors

## Troubleshooting

### Common Issues

1. **Port Configuration**
   - Railway automatically sets the `PORT` environment variable
   - Ensure your app listens on `0.0.0.0:$PORT`

2. **Environment Variables**
   - Double-check all required variables are set
   - Sensitive keys should be set in Railway dashboard, not in code

3. **Build Failures**
   - Check Railway build logs for dependency issues
   - Ensure `requirements.txt` is up to date

4. **Database Connection**
   - Verify database path and permissions
   - Consider using Railway's PostgreSQL for production

## Migration from Render

Key differences from your previous Render setup:

1. **Configuration**: `railway.json` instead of `render.yaml`
2. **Environment Variables**: Set via Railway dashboard or CLI
3. **Assets**: Railway handles static files automatically
4. **Domains**: Railway provides automatic HTTPS domains
5. **Scaling**: Railway offers automatic scaling options

## Cost Considerations

- Railway offers a generous free tier
- Pay-as-you-scale pricing model
- Monitor usage in the Railway dashboard

## Support

- Railway Documentation: [docs.railway.app](https://docs.railway.app)
- Railway Discord Community
- GitHub Issues for this project
