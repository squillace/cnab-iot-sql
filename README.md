# what you do
1. docker build --rm -f "cnab/Dockerfile" -t squillace/cnab-iot-sql:0.1.0 cnab && docker run -it --env-file deploy.64 squillace/cnab-iot-sql:0.1.0

2. az iot edge deployment list -g IoTEdgeResources -n cnab-hub --query '[].id' -o tsv | xargs -I {} az iot edge deployment delete -g IoTEdgeResources -d {} -n cnab-hub
