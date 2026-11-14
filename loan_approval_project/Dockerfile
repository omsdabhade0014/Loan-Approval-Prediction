# Container image for the web app. Works on Render, Railway, Fly.io, Google Cloud Run,
# Azure Container Apps, or any machine with Docker.
FROM python:3.13-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    DATA_DIR=/data

WORKDIR /app

# Install libraries first so this layer is cached between code changes
COPY requirements-app.txt .
RUN pip install --no-cache-dir -r requirements-app.txt

# Application code, templates, static files and the trained model
COPY app.py database.py features.py loan_model.joblib ./
COPY templates ./templates
COPY static ./static

# Folder for the history database. Mount a persistent disk here in production.
RUN mkdir -p /data
VOLUME ["/data"]

EXPOSE 8000

# Hosting platforms set PORT; default to 8000 when it is not set.
CMD ["sh", "-c", "gunicorn --workers 2 --bind 0.0.0.0:${PORT:-8000} --access-logfile - app:app"]
