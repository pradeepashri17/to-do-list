# to-do-list
# Django Todo Application

## Project Description

The Django Todo Application is a simple task management system built using Django. It allows users to create, read, update, and delete tasks. This application features basic CRUD operations and is designed to demonstrate Django's capabilities and how to deploy a Dockerized Django app on AWS Lambda.

## Features

- **Task Management**: Users can add, view, update, and delete tasks.
- **User Authentication**: Users can register, log in, and manage their accounts.
- **Search Functionality**: Search tasks by title or description.

## Installation and Running Locally

### Prerequisites

- **Python 3.9** or higher
- **pip** (Python package installer)
- **Docker** (for containerization)
- **AWS CLI** (for deployment)

### Steps to Run Locally

1. **Clone the Repository**

   ```bash
   git clone https://github.com/shreys7/django-todo.git
   cd django-todo
Install Dependencies
pip install -r requirements.txt
Apply Migrations
python manage.py migrate
Run the Development Server
python manage.py runserver
Dockerization
Dockerfile
Here is the Dockerfile used to containerize the application:
# Use the official Python image from the Docker Hub as the base image
FROM python:3.9-slim

# Set environment variables to avoid Python buffering and ensure output is logged immediately
ENV PYTHONUNBUFFERED=1

# Create and set the working directory inside the container
WORKDIR /app

# Copy the requirements file into the container
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code into the container
COPY . .

# Run Django migrations and collect static files
RUN python manage.py migrate
RUN python manage.py collectstatic --noinput

# Expose the port the app runs on
EXPOSE 8000

# Specify the command to run the application using Gunicorn
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "todo_project.wsgi:application"]
