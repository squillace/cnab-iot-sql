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

ENV APP_ID default
ENV PASSWORD default
ENV TENANT_DNS default
ENV CONDITION target
ENV HUB_NAME default
ENV DEVICE_ID default
ENV DEPLOYMENT_ID default
ENV AZURE_RESOURCE_GROUP default

COPY Dockerfile /cnab/Dockerfile
COPY app /cnab/app

CMD ["/cnab/app/run"]
