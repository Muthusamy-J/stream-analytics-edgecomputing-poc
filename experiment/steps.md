**Steps**:
Create an Azure Stream Analytics job to process data on the edge.
Connect the new Azure Stream Analytics job with other IoT Edge modules.
Deploy the Azure Stream Analytics job to an IoT Edge device from the Azure portal.

**Details:**

1. **Create a Linux VM and deploy Edge module** - https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux?view=iotedge-2020-11

2. **Create an Azure Stream Analytics job** to
      Receive data from your IoT Edge device.
      Query the telemetry data for values outside a set range.
      Take action on the IoT Edge device based on the query results.
     
3. Create a Azure storage account

4. Create a resource > Internet of Things > Stream Analytics job. Select "Edge" as hosting environment

5. Configure your job -> Under Job topology, select Inputs then Add stream input -> Choose Edge Hub from the drop-down list.
      Enter input and output alias by chossing Edge Hub ->
      Configure below Query : 
      ```
      SELECT 'reset' AS command
      INTO
      alert
      FROM
      temperature TIMESTAMP BY timeCreated
      GROUP BY TumblingWindow(second,30)
      HAVING Avg(machine.temperature) > 70
      ```
6. To prepare your Stream Analytics job to be deployed on an IoT Edge device, you need to associate the job with a storage account. When you go to deploy your job, the job definition is exported to the storage account in the form of a container. -> In Stream analytics job -> Under Configure, select Storage account settings then select Add storage account. -> Choose the Select Blob storage/ADLS Gen 2 -> Use the drop-down menus to select the Subscription and Storage account that you set -> Save

7. **Deploy the Job**: Going to deploy two modules. -> The first is SimulatedTemperatureSensor, which is a module that simulates a temperature and humidity sensor. -> The second is your Stream Analytics job. The sensor module provides the stream of data that your job query will analyze.

8. Go to IoT Hub -> IoT Edge -> Set modules -> 
Click Add and select IoT Edge Module.
For the name, type SimulatedTemperatureSensor.
For the image URI, enter mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0.

Add your Azure Stream Analytics Edge job with the following steps:

Click Add and select Azure Stream Analytics Module.
Select your subscription and the Azure Stream Analytics Edge job that you created.
Select Save.

9. Then, select Next: Routes to continue.
Replace the default route and upstream name and values with the pairs shown in following table. (modulename -> name of your Azure Stream Analytics module)
```
telemetryToCloud	FROM /messages/modules/SimulatedTemperatureSensor/* INTO $upstream
alertsToCloud	FROM /messages/modules/{moduleName}/* INTO $upstream
alertsToReset	FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint("/modules/SimulatedTemperatureSensor/inputs/control")
telemetryToAsa	FROM /messages/modules/SimulatedTemperatureSensor/* INTO BrokeredEndpoint("/modules/{moduleName}/inputs/temperature") 
```
Review + create -> create manifest

10. Go to Device details page. Select Refresh.

 see the new Stream Analytics module running, along with the IoT Edge agent and IoT Edge hub modules ( takes sometime )

10. Login to edge vm -> execute cmd  "iotedge logs SimulatedTemperatureSensor" -> see the sensor data and reset operation.

11. Configure Power BI as output of IoT Hub/ Iot edge hub and configure to see the data in Power BI
