---

copyright:
years: 2016, 2017
lastupdated: "2017-08-02"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Watson IoT Platform Gateway package (Beta)
{: #gw_package}

The {{site.data.keyword.iot_full}} Gateway package enables a {{site.data.keyword.iot_short_notm}} registered gateway to send events to {{site.data.keyword.iot_short_notm}} on behalf of devices that are in the resource group that is associated with the gateway.
{:shortdesc}

**Important:** The {{site.data.keyword.iot_short_notm}} Gateway package is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

## Overview

The {{site.data.keyword.iot_short_notm}} Gateway package includes the following entities:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/watson-iot/iot-gateway` | package | org, domain, gatewayTypeId, gatewayId, gatewayToken  | Work with {{site.data.keyword.iot_short_notm}} Gateway |
| `/watson-iot/iot-gateway/publishEvent` | action | org, domain, gatewayTypeId, gatewayId, gatewayToken, eventType, typeId, deviceId, payload | Send events from a registered gateway on behalf of its associated devices to {{site.data.keyword.iot_short_notm}}   |

## Creating a {{site.data.keyword.iot_short_notm}} Gateway package binding
To create a {{site.data.keyword.iot_short_notm}} Gateway package binding, you must specify the following parameters:

| Parameter |  Description |
| --- | ---  |
| org | The organization identifier |
| gatewayTypeId | The gateway type identifier of the registered gateway |
| gatewayId | The gateway identifier of the registered gateway |
| gatewayToken | The authorization token of the registered gateway |


Complete the following steps to create a package binding:  
1. [Login to the Bluemix console ![External link icon](../../../icons/launch-glyph.svg)](https://console.ng.bluemix.net/).
2. Create the [Internet of Things Platform Service](https://console.bluemix.net/docs/services/IoT/index.html) in Bluemix and [note the `API Key` and the `API Token`](https://console.bluemix.net/docs/services/IoT/platform_authorization.html#connecting-applications). This information is required to create a gateway type and to register a gateway.
3. [Create a gateway type](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html), for example, *myGWType* in your Watson IoT organization and [register an instance of the gateway](https://console.bluemix.net/docs/services/IoT/gateways/dashboard.html), for example, *myGWId*. Make a note of the *Gateway Token* for the registered gateway.
4. Create a package binding with the Watson IoT Platform Gateway package by using the following example command:
   ```
   wsk package bind /watson/iotgw myGW -p org myorg -p gatewayTypeId myGWType -p gatewayId myGWId -p gatewayToken myGWToken
   ```
5. Verify that the package binding exists by using the following command:  
   ```
   wsk package list
   ```

## Publishing Device Events

The `/watson/iotgw/publishEvent` action publishes events from a registered {{site.data.keyword.iot_short_notm}} gateway, on behalf of its associated devices. The following table describes the parameters that are used when publishing events:  

Parameter |  Description | Example
------------- | ------------- | -------------
org | The identifier of the organization to which the registered gateway belong.s  | `-p org "uguhsp"`
domain | Optional. The domain to which the registered gateway belongs. The default points to `messaging.internetofthings.ibmcloud.com` | `-p domain "messaging.internetofthings.ibmcloud.com"`
gatewayTypeId | The gateway type identifier of the registered gateway. | `-p gatewayTypeId "myGatewayType"`
gatewayId | The gateway identifier of the registered gateway. The identifier must be unique within an organization for a given gateway type. | `-p gatewayId "00aabbccde03"`
gatewayToken | The token (password) that is used by the registered gateway to connect to Watson IoT Platform.  | `-p gatewayToken "ZZZ"`
eventType | The event type that the registered gateway publishes events to on the behalf of its associated devices. | `-p eventType "evt"`
typeId | The device type of the device that is associated with the registered gateway. The registered gateway publishes events on behalf of the associated device. | `-p typeId "myDeviceType"`
deviceId | The device identifier of the device that is associated with the registered gateway. The registered gateway publishes events on behalf of the associated device. The device identifier must be unique within an organization for a given gateway type. | `-p deviceId "00aabbccde03_0001"`
payload | The payload that the registered gateway publishes on the behalf of the device. | `-p payload "{'d':{'temp':38}}"`


The following example shows how to publish events from the *iotgw* package:

Publish a device event by using the *publishEvent* action in the package binding that you created. You must replace `/myNamespace/myGateway` with your package name.

 ```
  wsk action invoke /myNamespace/myGateway/publishEvent -i --result --blocking -p org ORG_ID -p eventType value -p payload '{"test":"etsd"}' -p typeId myDeviceType -p deviceId 00aabbccde03_0001 -p gatewayTypeId myGatewayType -p gatewayId 00aabbccde03 -p gatewayToken "ZZZ"
 ```

 ## Working with the repository

Complete the following steps to deploy the package by using `installCatalog.sh`
1. `git clone https://github.com/ibm-watson-iot/openwhisk-package-watsoniotp`
2. `cd openwhisk-package-watsoniotp/packages`
3. `./installCatalog.sh AUTH APIHOST WSK_CLI`

where *AUTH* is your authorization key, *APIHOST* is the OpenWhisk hostname, and *WSK_CLI* is the location of the Openwhisk CLI binary.
