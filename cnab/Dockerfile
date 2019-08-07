FROM mcr.microsoft.com/azure-cli AS cli-base

RUN apk update
RUN apk add -u bash

 # Set the working directory to /app
#WORKDIR /app

# Copy the current directory contents into the container at /app
#COPY . /app

# Install any needed packages specified in requirements.txt
RUN az extension add --name azure-cli-iot-ext

COPY Dockerfile /cnab/Dockerfile
COPY app /cnab/app

CMD ["/cnab/app/run"]
