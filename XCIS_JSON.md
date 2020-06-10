# XCIS JSON Format

JSON is used to transfer data between the device, node, gateway then out to the web.

# Base Objects

## registered_device

`registered_device` - `{ "deviceName" = [ device_parameter[1], device_parameter[2], device_parameter[...], device_parameter[N] ] }`

where:
+ `deviceName` - The generic name of the device, eg. WaterTank. If nodes are re-named the node will reply its given name instead of the generic Node name.


Example:
+ `{ "Water Tank" = [ { "Level" = "50%", }, { "Volume" = "509", "U" = "L" }, { "Change" = [ { "height" = 1000 }, { "width" = 600 }, { "space" = 100 ] } ] }` - A Water tank level sensor which is currently at 50% and contains 509 L of water, whose sensor is mounted 1000 mm above the ground, the tank has a width (diameter) of 600 mm and the tank is considered full when there is a space of 100 mm below the sensor position.

## device_parameter

A device_parameter may be a device_function (input) or device_variable (output). An input is differentiated from an output because the data is an array of objects. When sending variable data you must ensure that the data is not sent as an array of objects because it will be interpreted incorrectly at by whoever is requesting data.

### device_function

if device_parameter = `device_function` the following syntax is used

`device_function` - `{ "functionName" = [ function_parameter[1], function_parameter[2], function_parameter[...], function_parameter[N] ] }`

where:
+ `functionName` - The name of a function which may be called by the user.

Example:

`{ "Change" = { "Tank height" = 1500 } }` where 1500 is the current tank height above the ground.

#### param_desc

`function_parameter` - `{ "parameterName" = defaultValue }`

Where:
+ `parameterName` - The name of a parameter which can be passed to a function as a request. This name should be descriptive of the variables purpose rather than reflective of the variables name in code as it is what would be displayed to the end user.
+ `defaultValue` - A JSON type which is not an array of objects. This should be a default value for a request, it could be the current value which is being modified or a default value which can be recognised as something to ignore.

Example:

`{ "Height" = 1500 }`

#### device_variable

** No Units **

`device_variable` - `{ "variableName" = variableValue }`

** With Units **

`device_variable` - `{ "variableName" = variableValue, "U" = "unitName" }`

Where:
+ `variableName` - The descriptive name of a variable
+ `variableValue` - 
+ `unitName` - a string representing a readable expression of the units

Examples:

`{ "Tank Height" = 2423 , "U" = "mm" }`
`{ "Gate Position" = "Open" }`

### gateway_request

## Gateway

Replies from the gateway are of the form of a series of [registered_device](#registered_device) objects.

JSON is obtained from:
+ /lora - Request objects cached by the Gateway
  + reply = [ node_device[1], node_device[2], node_device[...], node_device[N] ]
    + `node_device[k]` - the k-th node

+ /lora?id=k
  + reply = [ node_device[k] ]
    + `node_device[k]` - the k-th node

`{[registered_device[k]]}`

### node_device

`node_device` - ` [ registered_device[1], registered_device[2], registered_device[...], registered_device[N] ]

Where:
+ registered_device[1] - Node data
+ registered_device[k] - Data from the k-th device attached to Node


## Device


## Device
