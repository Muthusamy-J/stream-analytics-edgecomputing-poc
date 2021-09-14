Open Azure CLI,

az group create --name IoTEdgeResources --location westus2

**create IoT Hub**

az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 --partition-count 2

**Register an IoT Edge device with your newly created IoT hub.**

az iot hub device-identity create --device-id myEdgeDevice --edge-enabled --hub-name {hub_name}

az iot hub device-identity connection-string show --device-id myEdgeDevice --hub-name {hub_name}

**create vm and deploy edge runtime**

az deployment group create \
--resource-group IoTEdgeResources \
--template-uri "https://raw.githubusercontent.com/Azure/iotedge-vm-deploy/1.2.0/edgeDeploy.json" \
--parameters dnsLabelPrefix='<REPLACE_WITH_VM_NAME>' \
--parameters adminUsername='azureUser' \
--parameters deviceConnectionString=$(az iot hub device-identity connection-string show --device-id myEdgeDevice --hub-name <REPLACE_WITH_HUB_NAME> -o tsv) \
--parameters authenticationType='password' \
--parameters adminPasswordOrKey="<REPLACE_WITH_PASSWORD>"

