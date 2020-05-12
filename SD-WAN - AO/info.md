# SDWAN Workflow
## `workflow 1`

### `Action: Authetication`
You must first establish an HTTPS session to the server. To do this, you send a call to log in to the server with the following parameters: URL to send the request to — use `https://{vmanage-ip-address/j_security_check`, which performs the login operation and security check on the vManage web server at the specified IP address.  The API call payload—The payload contains the username and password in the format `j_username=username`& `j_password=password`.

### `Action: List Devices`
After we have established the HTTPS session, we can list the devices attached to the fabric, we use the call that retrieves a list of all devices in the network. To retrieve this list, use the following URL:` https://vmanage-ip-address/dataservice/device`.  In the templates table, the Device Templates column indicates how many device configuration templates are using a particular feature template,

### `Action: List Templates`
The next URL being called is URL: `https://{vmanage-ip-address/dataservice/template/feature` which show the devices in to which the feature template is deploy. 

### `Action: Attached Templates to devices`
Once the site/devices are identified we push and attach the feature template to the devices with URL: `https://{vmanage-ip-address/dataservice/template/device/config/attachfeature`  

### `Action: Validate Template deployment`
Validation of the feature template is completed by URL: `https://{vmanage-ip-address/dataservice/template/device/config/attached/[id]` validates which sites/device.
