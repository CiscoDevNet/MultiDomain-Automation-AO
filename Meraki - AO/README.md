# Meraki Workflow
Meraki networks are organized hierarchically.  Some actions can be taken in parallel if necessary, but some activities require the result of a previous call to be successful.  In this scenario, we’re going to add a branch network, create VLANs for that network, add site-to-site VPN settings, and, finally, configure the wireless SSID settings.  This will all be self-contained in one workflow in CAO that can then be reused as necessary.

## Configuration in CAO

Figure 1: Creating the Meraki Target


### Creating the Meraki Target
Before we get into the actual steps in the workflow, we need to set up Meraki as a Target within CAO. There is a pre-built Meraki family of activities in CAO, but it only covers a subset of the API calls that we need so in this instance we will set up an HTTP Endpoint.  Unfortunately, that also requires us to set the X-Cisco-Meraki-API-Key authorization header as a Global Variable rather than an Account Key so we can access it in our workflow.  Therefore the No Account Keys setting is set to True and the HTTP section will have HTTPS set with the HOST/IP ADDRESS set to api.meraki.com.


### Setting the X-Cisco-Meraki-API-Key
To complete the setup, in the Variables section we will set up a Global Variable for the X-Cisco-Meraki-API-Key and Organization ID* to be used in our API calls.  Both variables should be scoped globally.

*Note: The organization ID can be found by making an API call to https://api.meraki.com/api/v0/organizations.  We are using a static organization in this example but it can be grabbed dynamically in the workflow as well.

### Importing the workflows

Import Meraki-Net-Setup.json first then Meraki-Multi-Domain.json to create the following workflows.

### Creating the Network

Figure 2: Creating the Meraki Network

Figure 3: Getting the new network Id
Now that CAO is ready to go we can build out the workflow.  The first step is creating our new network in our organization.  This is done by calling the /networks endpoint with a method of POST using a Web Service core (pre-built) activity in CAO.  Note that the organization ID in the url is set by the global variable mentioned above using the notation “[$global.Meraki Org ID$]”.  A similar variable reference is made in the payload as the name of the network is being set dynamically through run-time input and is referenced as “[$workflow.Meraki Net Setup_Prod.input.Tenant Name]”.  Meraki Net Setup_Prod is the name of the workflow being created.  Creating a network generates a network ID that is needed in the following steps, so another CAO Core activity called JSONPath Query is used to parse the resulting JSON response body.  This can then be referenced in later actions.

### Claiming Devices

Figure 4: Claiming a device
Once the network is created, we need to claim the MX security appliance and MR access point that will be set up for the branch network.  This is also done using the Web Service activity but now pointing to the /devices/claim endpoint.  The network ID is pulled from the previously mentioned JSONPath Query activity and the serial numbers are statically passed in the body for this example.


Figure 5: Enabling VLANs
### Creating VLANs
Now that the devices are added to the network, the configuration for those devices can be set.  First, VLANs will be enabled and a new one added to the branch network.  This is done in two sequential Web Service activities.  The first calls /networks/<network id>/vlanEnabledState.  The second calls /networks/<network id>/vlans with the POST body containing the VLAN configuration.

### Setting Site-To-Site VPN

Figure 6: Converting the network JSON to a table

Figure 7: Checking for the network name “Headquarters” in the Condition Block activity

Figure 8: Making the /siteToSiteVpn API call
Once the VLANs are created, our new branch network can be set as a spoke for the Site-To-Site VPN.  This requires a reference the Headquarters network as a hub.  Therefore, we need to grab the network ID of the Headquarters network.  The first step in is using yet another Web Service activity to get the network list via the /networks endpoint.  The result of that API call is a JSON body with a list of networks and their attributes.  We need to parse the network ID of the network named Headquarters but converting the JSON into a table using CAO’s Read Table from JSON activity.  After the conversion, we can use CAO’s For Each activity to loop through the records until the name of the network equals Headquarters and the Condition Block activity is satisfied.  Then we can make another Web Service activity that performs a PUT to the /siteToSiteVpn resource, passing the necessary JSON body including the network ID of the hub that was pulled earlier.

 

 

### Setting SSID for client access
The final step to this workflow is to set the SSID for the Meraki branch network.  This is simply done by a Web Service activity (yes again!) that makes a PUT call to the first SSID managed for that network.  It will send the appropriate JSON body to configure per organization policy.


Figure 9: Setting the SSID
 

Success!
Now we have an easily repeatable method for deploying new branch networks that abstracts the need for admin access to Meraki and ONLY relies on a new name to be input for a new branch!
