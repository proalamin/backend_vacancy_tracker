# Pre-Deployment Checklist for Render.com

Use this checklist before deploying your Django app to Render.com:

## Code Preparation ✅

- [x] Updated `settings.py` with production-ready configuration
- [x] Added `whitenoise` for static file serving
- [x] Updated `requirements.txt` with all dependencies
- [x] Created `Procfile` with build and start commands
- [x] Created `.env.example` for environment variable documentation
- [x] Updated `.gitignore` to exclude sensitive files
- [x] Created `render.yaml` for infrastructure-as-code deployment

## Before Pushing to GitHub

- [ ] Never commit your `.env` file (add to .gitignore)
- [ ] Generate a secure SECRET_KEY: `python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"`
- [ ] Test locally with production settings:
  ```bash
  cp .env.example .env
  # Edit .env with test values
  pip install -r requirements.txt
  python manage.py collectstatic --noinput
  DEBUG=False python manage.py runserver
  ```
- [ ] Commit and push all changes to GitHub

## Render.com Configuration

- [ ] Create Render account at https://render.com
- [ ] Connect your GitHub repository
- [ ] Create Web Service (or use render.yaml for auto-deployment)
- [ ] Set environment variables:
  - `SECRET_KEY`: Use generated key (must be secure!)
  - `DEBUG`: Set to `False`
  - `ALLOWED_HOSTS`: Your Render domain
  - `DATABASE_URL`: If using PostgreSQL (create DB first)

## Database Setup

- [ ] Decide on database:
  - SQLite (default, but not recommended for production)
  - PostgreSQL on Render (recommended for free plan)
- [ ] If using PostgreSQL:
  - [ ] Create PostgreSQL database on Render
  - [ ] Copy connection string
  - [ ] Set as `DATABASE_URL` environment variable
- [ ] Migrations run automatically on deployment

## Initial Deployment

- [ ] Deploy from GitHub
- [ ] Wait for build to complete (3-5 minutes typical)
- [ ] Check Render logs for any errors
- [ ] Visit your live URL

## Post-Deployment Verification

- [ ] Access your app at `https://your-service.onrender.com`
- [ ] Test API endpoints
- [ ] Verify Django admin works (`/admin/`)
- [ ] Check static files load correctly
- [ ] Test database connectivity

## Optional - Custom Domain

- [ ] Purchase domain (if not already)
- [ ] Add domain in Render settings
- [ ] Update DNS records with Render values
- [ ] SSL certificate auto-configured

## Security Reminders

⚠️ **Critical:**

- Never commit `.env` file
- Use strong, unique `SECRET_KEY`
- Keep `DEBUG=False` in production
- Use HTTPS (auto-managed by Render)
- Regularly update dependencies for security patches

## Monitoring

- [ ] Set up Render alerts (optional)
- [ ] Monitor logs regularly
- [ ] Check for errors in Render dashboard

## Troubleshooting

If deployment fails:

1. Check Render logs for error messages
2. Verify all environment variables are set
3. Ensure `ALLOWED_HOSTS` includes your Render domain
4. Test locally with `DEBUG=False`
5. Check that `Procfile` is in root directory

## Useful Commands

```bash
# View Render logs
# (Use Render dashboard or CLI)

# Manually trigger redeploy
# (Use "Manual Deploy" button on Render)

# Clear static files and rebuild
python manage.py collectstatic --clear --noinput
```

---

**Need help?** See `RENDER_DEPLOYMENT.md` for detailed instructions.
