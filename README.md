# azure stream-analytics-edgecomputing-poc

**Edge real-time intelligence operations **

**Industrial IoT scenario:** Monitor the temperature of a device and control the device accordingly.

**PoC scope**: Collect the temperature streams from the sensors, process the streams in the edge, switch off the edge device/take the preventive measures when the temperature goes above a threshold and send the transformed data to cloud for analytics.

**PoC solution**: The solution is implemented with Azure IoT Edge Hub, Azure Stream Analytics Edge, Azure IoT Hub and Power BI.

**PoC implementation approach: **

• Azure VM is used as the edge device.

• Sensor simulator which generates the temperature data and Azure IoT Edge are deployed in 
Edge device

• Azure stream analytics edge module/job is pushed down into the edge device which has the 
implemented logic to calculate the average temperature over a rolling time interval window and 
the definition for the temperature threshold while monitoring the device

• When the temperature average reaches the threshold, the stream analytics edge sends an alert 
for the device to take the appropriate action. In this case, that action is to reset the simulated 
temperature sensor

• The transformed data (the timing of the sensor resets with the incoming alert data) is pushed to 
Azure IoT Hub and visualized in PowerBI

• In production, this functionality is to shut off a machine or take preventative measures when the 
temperature reaches dangerous levels
