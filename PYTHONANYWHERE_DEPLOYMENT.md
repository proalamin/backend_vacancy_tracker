# PythonAnywhere Deployment Guide

This project can run on PythonAnywhere as a Django web app.

## 1. Create app and virtualenv

Run these commands in a PythonAnywhere Bash console:

```bash
cd ~
git clone <your-repo-url> backend_vacancy_tracker
cd backend_vacancy_tracker
python3.13 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

## 2. Create `.env`

From project root:

```bash
cp .env.example .env
```

Then edit `.env` and set:

```env
SECRET_KEY=<a-strong-secret-key>
DEBUG=False
ALLOWED_HOSTS=yourusername.pythonanywhere.com
CSRF_TRUSTED_ORIGINS=https://yourusername.pythonanywhere.com
```

If you use PostgreSQL, also set:

```env
DATABASE_URL=postgresql://user:password@host:5432/dbname
```

If not set, SQLite is used by default.

## 3. Run migrations and collect static files

```bash
source ~/backend_vacancy_tracker/venv/bin/activate
cd ~/backend_vacancy_tracker
python manage.py migrate
python manage.py collectstatic --noinput
python manage.py createsuperuser
```

## 4. Configure PythonAnywhere Web app

1. Open the **Web** tab in PythonAnywhere.
2. Create a new web app:
   1. Domain: `yourusername.pythonanywhere.com`
   2. Manual configuration
   3. Python version: `3.13` (or closest available)
3. Set virtualenv path:
   1. `/home/yourusername/backend_vacancy_tracker/venv`
4. Set source code path:
   1. `/home/yourusername/backend_vacancy_tracker`
5. Set static files mapping:
   1. URL: `/static/`
   2. Directory: `/home/yourusername/backend_vacancy_tracker/staticfiles`

## 5. Update WSGI file

In the PythonAnywhere Web tab, open the generated WSGI config file and replace content with:

```python
import os
import sys

path = '/home/yourusername/backend_vacancy_tracker'
if path not in sys.path:
    sys.path.insert(0, path)

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'backend_vac_tr_main.settings')

from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()
```

## 6. Reload and verify

1. Click **Reload** in Web tab.
2. Check:
   1. `https://yourusername.pythonanywhere.com/api/v1/jobs/`
   2. `https://yourusername.pythonanywhere.com/api/docs/`
   3. `https://yourusername.pythonanywhere.com/admin/`

## 7. Troubleshooting

- 400 Bad Request (Invalid host header):
  - Ensure `ALLOWED_HOSTS` includes `yourusername.pythonanywhere.com`.
- CSRF errors in admin:
  - Ensure `CSRF_TRUSTED_ORIGINS` includes `https://yourusername.pythonanywhere.com`.
- Static files not loading:
  - Re-run `collectstatic` and verify static mapping in Web tab.
- Import/module errors:
  - Confirm web app virtualenv points to `/home/yourusername/backend_vacancy_tracker/venv`.
