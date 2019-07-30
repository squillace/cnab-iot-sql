FROM mcr.microsoft.com/azure-cli AS cli-base

RUN apk update
RUN apk add -u bash

 # Set the working directory to /app
#WORKDIR /app

# Copy the current directory contents into the container at /app
#COPY . /app

# Install any needed packages specified in requirements.txt
RUN az extension add --name azure-cli-iot-ext

# Make port 80 available to the world outside this container
# EXPOSE 80

# Define environment variable

ENV APP_ID http://demo-cli-login
ENV PASSWORD e3a5b489-f18b-47a0-a6f5-ea7b8cc5b702
ENV TENANT_DNS microsoft.onmicrosoft.com
ENV CONDITION target
ENV DEPLOYMENT default

COPY Dockerfile /cnab/Dockerfile
COPY app /cnab/app

CMD ["/cnab/app/run"]
