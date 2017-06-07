# node-red-contrib-tradfri
Node-RED nodes to communicate with IKEA TRÅDFRI lights. [![npm version](https://badge.fury.io/js/node-red-contrib-tradfri.svg)](https://badge.fury.io/js/node-red-contrib-tradfri)

Two, really easy to use, nodes that will help you to:
 - Get information on all devices and or groups
 - Set the state - on/off - on devices or groups
 - Set the brightness on devices or groups
 - Set the color on devices

![IKEA TRÅDFRI devices](https://cloud.githubusercontent.com/assets/2181965/26756721/7c3cddac-48a9-11e7-83fb-701d3c111e4b.jpg)

## Documentation

### tradfri-out node
Set state (on/off/brightness) on an IKEA TRÅDFRI device or group

#### Inputs

<dl class="message-properties">

<dt>payload <span class="property-type">object | string</span></dt>

<dd>either a string with `on` or `off` or an object describing the action</dd>

</dl>

#### Outputs

1.  Standard output

    <dl class="message-properties">

    <dt>payload <span class="property-type">boolean | optional</span></dt>

    <dd>if output is enabled success or failure is described by true or false</dd>

    </dl>

#### Details

If `msg.payload` is a a string (on/off) a device or group must have been defined in the node. If `msg.payload` is an object any information will override defined attributes in the node.
**The following attributes of the object are available:**
 - `tradfri_id` (Integer) - optional - Not needed if target has been specified in the node
 - `type` (String) - optional - ['group', 'device'] - specifies if the call is for a group or device. Not needed if target has been specified in the node
 - `state` (String) - ['on', 'off', 'toggle'] - specifies the state instruction
 - `brightness` (Integer) - [0-255] - specifies the brightness of the instruction
 - `color` (String) - [hex color value, 'cool', 'normal', 'warm'] - color code (HEX - e.g. 'ffffff' or cool/normal/warm)
 - `transitiontime` (Integer) - time in seconds to transition from one state to another

**Examples:** `msg.payload = 'on';` or `msg.payload = { tradfri_id: 1234, type:'device', state: 'on', color: 'cool'}`

**DEPRECATED**: msg.payload.instruction variable is deprecated from v1.0.5
~~`{ tradfri_id: 12321, type: "group", instruction: { state: "on", brightness: 255}}` where `tradfri_id`, `type` ('device' or 'group') and `brightness` are optional parameters (depending on the node configuration)~~

### tradfri-get node
Retrieves IKEA TRÅDFRI device information from your IKEA hub.

#### Inputs

<dl class="message-properties">

<dt>payload <span class="property-type">string | integer | optional</span></dt>

<dd>the device or group id to lookup</dd>

</dl>

#### Outputs

1.  Standard output

    <dl class="message-properties">

    <dt>payload <span class="property-type">object | array</span></dt>

    <dd>Device, group object or an array with all groups and devices</dd>

    </dl>

#### Details

If `msg.payload` is a number the details of that device or group will be retrieved and returned in msg.payload as an object. If `msg.payload` is not set or is not a number the complete structure will be retrieved from the defined IKEA Hub.


### tradfri configuration node
The `Config` setting requires a number of parameters:

*   `Security Code` - As found on the sticker on the bottom of the IKEA TRÅDFRI hub
*   `IP address` - The IP address of the IKEA TRÅDFRI hub
*   `CoAP path` - The file system path to the CoAP library that communicates with the IKEA TRÅDFRI hub. A set of precompiled versions of libcoap is available to choose from. If they don't work or you want to be sure to use the latest version, please follow the instructions to compile from the Github repo of libcoap.

Look at [node-tradfri-argon](https://github.com/nidayand/node-tradfri-argon) on instructions on how to get or compile the libcoap library. The executable will be in the [examples] directory when it has finished the compilation. E.g. `/home/pi/libcoap/examples/coap-client`
