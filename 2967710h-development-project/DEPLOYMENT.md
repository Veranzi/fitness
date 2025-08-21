# Deployment Guide for Render

This guide will help you deploy your Django Fitness Journal application to Render.

## Prerequisites

- A Render account (free tier available)
- Your code pushed to a Git repository (GitHub, GitLab, etc.)

## Deployment Steps

### 1. Connect Your Repository

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click "New +" and select "Web Service"
3. Connect your Git repository
4. Select the repository containing your Django app

### 2. Configure the Web Service

**Basic Settings:**
- **Name**: `fitness-journal` (or your preferred name)
- **Environment**: `Python 3`
- **Region**: Choose closest to your users
- **Branch**: `main` (or your default branch)

**Build & Deploy Settings:**
- **Build Command**: `pip install -r requirements.txt`
- **Start Command**: `gunicorn fitness_journal.wsgi:application`

### 3. Environment Variables

Render will automatically set these:
- `SECRET_KEY`: Auto-generated
- `DATABASE_URL`: Auto-generated when you create a database
- `DEBUG`: Set to `False` for production

You can add custom environment variables in the Render dashboard if needed.

### 4. Create PostgreSQL Database

1. In Render dashboard, click "New +" and select "PostgreSQL"
2. Name it `fitness-journal-db`
3. Choose the free plan
4. Select the same region as your web service
5. Render will automatically link the database to your web service

### 5. Deploy

1. Click "Create Web Service"
2. Render will automatically build and deploy your app
3. The first deployment may take 5-10 minutes

## File Structure for Deployment

Your project should have these files in the root directory:
```
2967710h-development-project/
├── requirements.txt          # Python dependencies
├── render.yaml              # Render configuration
├── build.sh                 # Build script
├── Procfile                 # Alternative deployment config
├── runtime.txt              # Python version
├── manage.py                # Django management
├── fitness_journal/         # Django project settings
├── static/                  # Static files
├── frontend/                # Frontend templates
└── [your Django apps]/
```

## Post-Deployment

### 1. Run Migrations

After first deployment, you may need to run migrations:
1. Go to your web service in Render dashboard
2. Click "Shell" tab
3. Run: `python manage.py migrate`

### 2. Create Superuser (Optional)

If you need admin access:
1. In the Shell tab, run: `python manage.py createsuperuser`
2. Follow the prompts

### 3. Collect Static Files

Static files are automatically collected during build, but you can manually run:
```bash
python manage.py collectstatic --no-input
```

## Troubleshooting

### Common Issues

1. **Build Failures**: Check the build logs in Render dashboard
2. **Database Connection**: Ensure `DATABASE_URL` is set correctly
3. **Static Files**: Verify `STATIC_ROOT` and WhiteNoise configuration
4. **Environment Variables**: Check all required variables are set

### Logs

- View build logs in the Render dashboard
- Check application logs for runtime errors
- Use Render's built-in logging features

## Security Notes

- `DEBUG` is automatically set to `False` in production
- `SECRET_KEY` is auto-generated and secure
- HTTPS is automatically enabled
- Security headers are configured in production

## Cost

- **Free Tier**: 750 hours/month for web services
- **Free Tier**: 90 days for PostgreSQL databases
- **Upgrade**: Available if you need more resources

## Support

- [Render Documentation](https://render.com/docs)
- [Render Community](https://community.render.com/)
- [Django Deployment Checklist](https://docs.djangoproject.com/en/5.2/howto/deployment/checklist/)