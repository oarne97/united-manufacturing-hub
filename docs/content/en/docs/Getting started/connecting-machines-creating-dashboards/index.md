
---
title: "2. Connecting machines and creating dashboards"
linkTitle: "2. Connecting machines and creating dashboards"
weight: 2
description: >
  This section explains how the United Manufacturing Hub is used practically 
---

# Prologue: What do you want to implement?

Before you start, you should think about **what** data you want to collect, **how** you want to do it and **why**.
Do you want to make individual process values available to a higher-level information system (temperature, vibration strength, distances, switching states, etc.) or do you want to determine the status of a machine (when is the machine running, when is it not and if it is not, why is it not) or do you want to collect product-related information to form a digital shadow. Many roads lead to Rome, the important thing here is to choose the use case first and then the appropriate tools.

In the following, we will describe how you can determine the status of a machine and transmit individual process values with the help of the United Manufacturing Hub.

# Prerequisites:

To use this guide, you should meet the following requirements:

**General**:
- Connection to the Edge PC and the Node-RED instance running on it (accessible under the IP address of the Edge PC port 1880).
- Connection to the edge PC and the MQTT broker instance running on it (accessible under the IP address of the edge PC port 1883).
- For a retrofit: Connected sensors
- When connecting a PLC: Connected PLC

**Software necessary**: 
- MQTT Explorer to view messages in real time: http://mqtt-explorer.com/ 
- Factorycube-edge and Factorycube-server fully installed and configured.

**Software optional**:
- UA Expert to read OPC/UA interfaces: https://www.unified-automation.com/de/downloads/opc-ua-clients.html 

# Inputs

In order to transmit information to the United Manufacturing Hub, the data points must first be extracted from various data sources. To do this, you should first familiarise yourself with Node-RED, as we will use this programme in the following to connect various data sources. If you haven't worked with node-red yet, [here](https://nodered.org/docs/user-guide/) is a good documentation.

## Node-RED data sources/nodes 
Node-RED provides so-called nodes directly or via plugins that allow the user to connect various data sources:

- OPC/UA ([documentation for this node](https://flows.nodered.org/node/node-red-contrib-opcua))
- Siemens S7 ([documentation for this node](https://flows.nodered.org/node/node-red-contrib-s7))
- TCP/IP ([documentation for this node](https://flows.nodered.org/flow/bed6f676d088670d7e1bc298943338b5))
- Rest API  ([documentation for this node](https://cookbook.nodered.org/http/create-an-http-endpoint))
- Modbus  ([documentation for this node](https://flows.nodered.org/node/node-red-contrib-modbus))
- MQTT ([documentation for this node](https://cookbook.nodered.org/mqtt/))

The following describes how to use these data sources/nodes using the OPC/UA node as an example. 

## United Manufacturing Hubs sensorconnect
One type of data source is the sensorconnect software package from the United Manufacturing Hub, which can be used to connect and read out IO-Link sensors quickly and easily.

Here, values from IO-Link sensors are read out with the help of an IO-Link gateway and then made available via [MQTT](http://www.steves-internet-guide.com/mqtt-works/).

To get a quick and easy overview of the available MQTT messages and topics we recommend the [MQTT Explorer](http://mqtt-explorer.com/). If you don't want to install any extra software you can use the MQTT-In node to subscribe to all available topics by subscribing to  `#` and then direct the messages of the MQTT in nodes into a debugging node. You can then display the messages in the nodered debugging window and get information about the topic and available data points.


Topic structure: `ia/raw/<transmitterID>/<gatewaySerialNumber>/<portNumber>/<IOLinkSensorID>`

**Example for ia/raw/**

Topic: `ia/raw/2020-0102/0000005898845/X01/210-156`

This means that the transmitter with the serial number `2020-0102` has one ifm gateway connected to it with the serial number `0000005898845`. This gateway has the sensor `210-156` connected to the first port `X01`.

```json
{
"timestamp_ms": 1588879689394, 
"distance": 16
}
```

## How to use data sources in concrete terms: 

### Example 1: Use of MQTT and sensorconnect (among other things for retrofitting)

**Step 1: Connect sensors and gateways to the Factorycube/Edge PC**

Connect an IO-Link sensor to an IFM IoT/IO-Link Gatway (e.g. Al1350, Al1352).
Connect the IO-Link gateway to the edge PC on which the factorycube edge stack is installed. Make sure that the IO-Link gateway has an IP address via DHCP that matches the IP address range of the helm values.yaml file.

**Step 2: Connect to an MQTT Broker with the help of the MQTT Explorer**

In the second step you should connect to the MQTT broker from which you want to pull the data and get an overview of the available data points.

**Step 3: Identify the topic and the information you want to extract**

 As an end result you should have 2 things, 1. the MQTT topic under which the data you are interested in is sent, 2. the "key" within the JSON you are interested in, such as the temperature.
 
**Step 4: Connect to the MQTT broker within Node-RED**

**Step 5: Extract the information relevant to you from the message**

ToDo: Add Picture and Example Flow
### Example 2: Use of OPC/UA

**Step 1: Connect to the OPC/UA endpoint using UA-Expert**

First of all: [Here](http://documentation.unified-automation.com/uaexpert/1.4.0/html/first_steps.html) is a guide on how to use UA-Expert.



**Step 2: Identify the tag/data point you want to extract using UA-Expert**

**Step 3: Connect to the OPC/UA endpoint within Node-RED**

**Step 4: Extract the information relevant to you from the message**

ToDo: Add Picture and Example Flow
# Connect machines
# 1. How to determine the machine status and thus enable a performance management dashboard.

In order to calculate the OEE and other KPIs, the system needs the following information:
- When is the machine running, when is it not running and if the machine is not running, information on why it is not running.
- When are productive times (shifts scheduled)
- Optional: How much is produced.

Based on this information, the United Manufacturing Hub determines the machine-relevant key figures and then serves them via a backend.

Our standard flow is always a good starting point and you can find it [here](https://github.com/united-manufacturing-hub/united-manufacturing-hub/blob/b16dec6e6b4ec076b87c26e88f647a6385ac0ca4/deployment/factorycube-edge/values.yaml#L58) (formatted as JSON):  

**Important information before you start**

- 
-
-
-

## How to tell if the machine is running, when it is not running and if not, information about why it is not running.
The target variable to be determined is the current state of the machine, the topic "/state". To make this easier to calculate, we have the two connectors "activity" and "detectedAnnomaly" as auxiliary variables. However, according to our data model, messages can also be written directly into the topic /state and the topics /activity and /detectedAnnomaly can be ignored.

**In the following, we will describe the path via the auxiliary variables /activity and /detectedAnomaly to determine the /state of the machine.**

### Connector /activity: Is the machine running or not:

## How to determine when when are productive times (shifts scheduled):

## Optional: How to determine how much is produced:

# 2. Display process values in the dashboard and make them available to the information system
Important the Topic /processValue Topic and the processValue node accepts only integer or float as data type


# Create dashboards

