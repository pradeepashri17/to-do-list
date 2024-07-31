
### `README.md`

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
   ```

2. **Install Dependencies**

   ```bash
   pip install -r requirements.txt
   ```

3. **Apply Migrations**

   ```bash
   python manage.py migrate
   ```

4. **Run the Development Server**

   ```bash
   python manage.py runserver
   ```

   Access the application at `http://localhost:8000`.

## Dockerization

### Dockerfile

Here is the `Dockerfile` used to containerize the application:

```Dockerfile
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
```

### Build and Run Docker Container

1. **Build the Docker Image**

   ```bash
   docker build -t my-django-todo-app .
   ```

2. **Run the Docker Container**

   ```bash
   docker run -p 8000:8000 my-django-todo-app
   ```

   Access the application at `http://localhost:8000`.

## Deployment to AWS Lambda

### Create an Amazon ECR Repository

1. **Open the Amazon ECR Console**: [Amazon ECR Console](https://console.aws.amazon.com/ecr/).
2. **Create a New Repository**: Name it `my-django-todo-repo` and create it.

### Push Docker Image to Amazon ECR

1. **Authenticate Docker to Your ECR**

   ```bash
   aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <your-account-id>.dkr.ecr.<your-region>.amazonaws.com
   ```

2. **Tag and Push Docker Image**

   ```bash
   docker tag my-django-todo-app:latest <your-account-id>.dkr.ecr.<your-region>.amazonaws.com/my-django-todo-repo:latest
   docker push <your-account-id>.dkr.ecr.<your-region>.amazonaws.com/my-django-todo-repo:latest
   ```

### Create and Configure AWS Lambda Function

1. **Open AWS Lambda Console**: [AWS Lambda Console](https://console.aws.amazon.com/lambda/).
2. **Create a New Function**: Choose **"Container image"**, provide a name, and use the image URI from ECR.

1. **Open Amazon API Gateway Console**: [API Gateway Console](https://console.aws.amazon.com/apigateway/).
2. **Create a New API**: Choose **"HTTP API"** and configure routes to integrate with your Lambda function.

### Test the Deployment

- Access the application via the API Gateway URL and verify its functionality.
