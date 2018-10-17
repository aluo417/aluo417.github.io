---
title: Docker Tutorial
description: how to use docker to wrap up working environment
categories:
 - notes
tags: Docker, tutorial
---

> Build an app the Docker way! No worry about **Environment** setup on different machines. Content extracted from [Docker Doc](https://docs.docker.com/get-started/part2/#dockerfile).

<!-- more -->

## Define a container with `Dockerfile`
Just like other 'setting' files, `Dockerfile` defines how the app runs in such environment.

A `Dockerfile` is placed into the working directory as followings:

{% highlight config %}
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
{% endhighlight %}
