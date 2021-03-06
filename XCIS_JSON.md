# XCIS JSON Format

JSON is used to transfer data between the device, node, gateway then out to the web.

# Base Objects

## registered_device

registered_device - `{ "deviceName" : { device_object[1], device_object[2], device_object[...], device_object[N] } }`

where:
+ `deviceName` - The generic name of the device, eg. WaterTank, Node.

Example:
+ `{ "Water Tank" : { "Level" : "50%", }, { "Volume" : "509", "U" : "L" }, { "Change" : [ { "height" : 1000 }, { "width" : 600 }, { "space" : 100 ] } ] }` - A Water tank level sensor which is currently at 50% and contains 509 L of water, whose sensor is mounted 1000 mm above the ground, the tank has a width (diameter) of 600 mm and the tank is considered full when there is a space of 100 mm below the sensor position.

## device_object

device_object - `"objectName" : { device_parameter[1], device_parameter[2], device_parameter[...], device_parameter[N] }`

where:
+ `objectName` - The generic name for a functional module within the device, eg. Temp, Motor, Gate 

Example:
+ `"Temp" : { ... }`
+ `"Motor" : { ... }`
+ `"Gate" : { ... }`

## device_parameter

A device_parameter may be a device_function (input) or device_variable (output). An input is differentiated from an output because the data is an array of objects. When sending variable data you must ensure that the data is not sent as an array of objects because it will be interpreted incorrectly at by whoever is requesting data.

### device_function

if device_parameter = `device_function` the following syntax is used

`device_function` - `{ "functionName" : { function_parameter[1], function_parameter[2], function_parameter[...], function_parameter[N] } }`

where:
+ `functionName` - The name of a function which may be called by the user.

Example:
+ `{ "pump" : { "power" : 60 }, { "timer" : 100 } }` - There is a pump, which is set to power of 60% and a timer with 100s remaining, however both these values could be modified if the user chooses.

#### function_parameter

function_parameter - `{ "parameterName" : defaultValue }`

Where:
+ `parameterName` - The name of a parameter which can be passed to a function as a request. This name should be descriptive of the variables purpose rather than reflective of the variables name in code as it is what would be displayed to the end user, the units, if not obvious, should be implied by the name.
+ `defaultValue` - A JSON type which is not an array of objects. This should be a default value for a request, it could be the current value which is being modified or a default value which can be recognised as something to ignore.

Example:
+ `{ "Height" : 1500 }`

#### device_variable

device_variable - `{ "variableUnits" : variableValue }`

Where:
+ `variableUnits` - a string representing a readable expression of the units
+ `variableValue` - a non-object JSON value or array of non-object, non-array values

Examples:
+ `{ "mm" : 2423 }` - a length of 2423 mm
+ `{ "m" : [ 16.2, 32.6] }` - position in (x,y) co-ordinates

### gateway_request

gateway_request = `{ "node" : NodeID, "device" : DeviceID, "functionName" : [ param[1], param[2], param[...], param[N] ] }`

Where:
+ `NodeID` - The ID number of the node being addressed
+ `DeviceID` - The position along the string of devices being addressed
+ `functionName` - a string representing the name of the function being requested
+ `param[k]` - a variable which represents the k-th parameter

functionName and param[s] should be described by a [device_function](#device_function).

Example:
Consider a water tank sensor with the following [device_function](#device_function): `{ "Change" = { { "height" = 1000 }, { "width" = 600 }, { "space" = 100 ] } } }`


Perhaps a farmer installs an overflow to their tank 200 mm below the lid, now the tank will never reach 100 mm below. The farmer requests the water tank sensor "space" be set to 200 instead of 100. An example request should be formed like (numbers are made up):

`{ "node" : 4, "device" : 2, "Change" : { { "height" : 1000 }, { "width" : 600 }, { "space" : 200 } } }`


## Gateway

Web replies from the gateway are of the form of a series of [registered_device](#registered_device) objects.

JSON is obtained from:
+ /lora - Request objects cached by the Gateway
  + reply = `[ node_device[1], node_device[2], node_device[...], node_device[N] ]`
    + `node_device[k]` - the k-th node

+ /lora?id=k
  + reply = `[ node_device[1] ]`
    + `node_device` - the k-th node as the first entry
    
+ /lora?id=k,dev=n
  + reply = `[ [ registered_device[1] ] ]`
    + `node_device[k]` - the k-th node's n-th attached devices as the first entry.

### node_device

node_device - `[ registered_device[1], registered_device[2], registered_device[...], registered_device[N] ]`

Where:
+ registered_device[1] - Node data
+ registered_device[k] - Data from the k-th device attached to Node

### LoRa

Coming Soon - more will be updated as needed
