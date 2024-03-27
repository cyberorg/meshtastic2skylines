# Meshtastic2skylines Node-RED Flow

## Overview

This Node-RED flow is designed to forward position data to [https://skylines.aero](https://skylines.aero) using data from an MQTT broker using livetrack24 protocol. It includes functionalities to ingest data from MQTT, decode the incoming packets, deduplicate packets, and send position measurements to Skylines via HTTP POST requests.

## Subflows

### Ingest from MQTT
This subflow handles the ingestion of data from MQTT. It subscribes to all topics ("#") with a quality of service (QoS) of 2. If you do not want to forward data to mqtt.meshtastic.org, disable the connection from the MQTT In node to the MQTT Out node.

### Measurement: Position
This subflow is responsible for generating position measurements from the decoded packet data. It converts latitude, longitude, altitude, time, satellites in view, ground speed, and ground track into a structured payload suitable for storage or further processing.

### Filtercrap (Function)
This function node filters out invalid messages and adds types to valid messages. It checks for valid latitude and longitude values in the payload.

### Generate Position Measurement (Function)
This function node generates position measurements from the decoded packet payload. It calculates session IDs and prepares data for HTTP POST requests to Skylines.

### HTTP Request (HTTP Request)
This node sends HTTP POST requests to https://skylines.aero with the position measurement data.

### Debug Nodes
Debug nodes are included for debugging purposes. They can be activated or deactivated as needed.

## Additional Information
- **mqtt in and out**: Modify to point to your mqtt server in Ingest subflow. Disable link to mqtt out if you do not wish to forward to mqtt.meshtastic.org.
- **Skylines Integration**: Ensure that the user mapping is correctly set in the "toskylines" function to map user IDs to session IDs for Skylines integration. Update the `userConfig` object with your own device IDs and Skylines tracking IDs.
```javascript
// Define a configuration object mapping user IDs to session IDs
var userConfig = {
    "!YOURDEVICE1": "SLTRACKINGID1",
    "!YOURDEVICE2": "SLTRACKINGID2"
};
```

For more information about Skylines, visit [Skylines GitHub Repository]([https://github.com/skylines/aero](https://github.com/skylines-project/skylines)).

**Dependencies**: This flow utilizes the Meshtastic Node-RED contrib nodes. For installation and usage instructions, refer to [Meshtastic Node-RED contrib](https://flows.nodered.org/node/@meshtastic/node-red-contrib-meshtastic) on Node-RED flows.

**Based on**: This flow is based on the Meshtastic Node-RED contrib nodes. For more information and updates, see [Meshtastic Node-RED contrib GitHub repository](https://github.com/scruplelesswizard/meshtastic-node-red).

Ensure all necessary dependencies are installed and configured properly in your Node-RED environment.

**Acknowledgement**: Special thanks to ChatGPT for being fantastic!
#meshtastic #paragliding #skylines #flyxc.app #puretrack.io #livetrack24
