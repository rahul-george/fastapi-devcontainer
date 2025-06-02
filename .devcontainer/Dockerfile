# Use an official Python base image
FROM python:3.11-slim

# Set working directory
WORKDIR /workspace

# Install FastAPI and Uvicorn
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Install common dev tools (optional)
RUN pip install black isort mypy

# Default command
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
