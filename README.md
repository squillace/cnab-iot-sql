# what you do
1. docker build --rm -f "cnab/Dockerfile" -t squillace/cnab-iot-sql:0.1.0 cnab && docker run -it --env-file deploy.64 squillace/cnab-iot-sql:0.1.0