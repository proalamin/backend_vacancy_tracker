# Quick Start: Deploy to Render.com

## 5-Minute Setup

### 1. Prepare Your Code

```bash
# Install new dependencies locally
pip install -r requirements.txt

# Test with production settings
DEBUG=False python manage.py collectstatic --noinput

# Commit changes
git add .
git commit -m "Configure for Render deployment"
git push origin main
```

### 2. Create Render Account

- Go to https://render.com
- Sign up with GitHub
- Click "New +" → "Web Service"
- Select your repository

### 3. Configure Service (Fill in Render Form)

```
Name: vacancy-tracker-backend
Environment: Python
Build Command:
  pip install -r requirements.txt && python manage.py collectstatic --noinput && python manage.py migrate

Start Command:
  gunicorn backend_vac_tr_main.wsgi:application

Plan: Free
```

### 4. Add Environment Variables (in Render Dashboard)

Click "Environment":

```
SECRET_KEY=<generate new one>
DEBUG=False
ALLOWED_HOSTS=your-service.onrender.com
```

For Database (optional):

```bash
# Generate SECRET_KEY locally first:
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

### 5. Deploy

Click "Create Web Service" and wait 3-5 minutes.

Your app will be live at: `https://your-service-name.onrender.com`

---

## What's Configured Automatically

✅ Gunicorn web server
✅ WhiteNoise for static files
✅ Database migrations on deploy
✅ SSL/HTTPS certificate
✅ Security headers
✅ Debug mode disabled in production

## Next Steps

1. **Test your API**: Visit `https://your-service.onrender.com/api/v1/`
2. **Access Admin**: Visit `https://your-service.onrender.com/admin/`
3. **Add Database** (optional): Use PostgreSQL for better reliability
4. **Custom Domain** (optional): Add your own domain

---

**Full Guide**: See `RENDER_DEPLOYMENT.md`
**Checklist**: See `DEPLOYMENT_CHECKLIST.md`
