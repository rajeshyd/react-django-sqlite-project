# Use an official Python runtime as a parent image
FROM python:3-alpine

ENV PYTHONUNBUFFERED 1


RUN apk update \
 && apk add gcc python3-dev

# Allows docker to cache installed dependencies between builds
COPY requirements.txt requirements.txt
RUN pip install --upgrade pip
RUN pip install --no-cache-dir -r requirements.txt



# Mounts the application code to the image
COPY . app
WORKDIR /app


RUN python3 manage.py makemigrations
RUN python3 manage.py migrate

EXPOSE 8000

# runs the production server
CMD ["python3", "manage.py","runserver", "0.0.0.0:8000"]




