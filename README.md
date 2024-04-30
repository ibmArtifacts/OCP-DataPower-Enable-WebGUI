# API Connect on OpenShift (OCP) DataPower: Enable WebGUI  
  
The IBM DataPower Gateway WebGUI is not enabled by default on OpenShift.  
Keep in mind, that even if the webgui is enabled, anything that is changed, configured, or added to the gateway from the webgui will be wiped out since it is managed by the custom resource DataPower operator.  
  
To enable the webgui you must have the following pre-reqs:  
- Access to the OpenShift cluster namespace and be able to update the gateway cluster CR.  
- Privileges to create a route for the webgui.


1. Naviagte to the APIC namespace where the gateway is deployed to and go into the Installed Operators > ibm-connect operator > All instances tab > apic-gw > inside the yaml, update the webGUIManagementEnabled to **true**.  
![image](https://github.com/ibmArtifacts/OCP-DataPower-Enable-WebGUI/assets/66093865/5f4a67df-d039-421d-aa3a-c07d35d7dd71)

2. Once that is enabled, create a route for the WebGUI:  
```
oc create route passthrough datapower-ui --service=apic-gw-datapower --port=9090
```

3. Once the route is created, you may find the endpoint with `oc get route`.
4. The following will get you the password for the admin login:
```  
oc get secret apic-gw-admin -o jsonpath="{.data.password}" | base64 -d
```
