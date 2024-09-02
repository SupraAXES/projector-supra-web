# projector-supra-web

**project-supra-web** is a customized projector image for [**SupraRBI-VNC**](https://github.com/supraaxes/suprarbi-vnc) and some other solutions from [SupraAXES Technologies, Inc.](https://www.supraaxes.com). 

> ```
> docker pull supraaxes/projector-supra-web
> ```

- private chromium-based enterprise browser with customized UI for better user experience
    - context menu for copy/paste, reload/forward/back, etc.
    - dialog box for upload and download
    - multiple tabs
    - download manager
- autofill

## License file
**projector-supra-web** is a commercial image with an evaluation license for 4 monthes. There will be monthly update to the official image, identified by specific tag (e.g. supraaxes/projector-supra-web:202409) and with up-to-date evaluation license.

A license file is required for each business deployment, please contact info@supraaxes.com for detailed information.


## Usage
To use **projector-supra-web** for [**SupraRBI-VNC**](https://github.com/supraaxes/suprarbi-vnc), please make sure the environment variable *SUPRA_PROJECTOR_IMAGE* is set to *projector-supra-web* for the **SupraRBI-VNC** server.

```
docker run --name rbi-vnc -d \
    --network supra-projector \
	-p 5900:5900 \
	-e SUPRA_RBI_NAME='rbi-vnc' \
	-e SUPRA_PROJECTOR_NETWORK='supra-projector' \
	-e SUPRA_PROJECTOR_IMAGE='supraaxes/projector-supra-web' \
	-v /var/run/docker.sock:/var/run/docker.sock \
	vnc_rbi
```

### username for VNC connection
The target URL specified in ** username for VNC connection** MUST be the same as the value of **url** in the **JSON file with autofill settings**.

### password for VNC connection
```
{
...
    "mounts":[
      "/autofill/settings/file.json:/opt/supra/conf/autofill.json:ro",
      "/customer/license.json:/opt/supra/conf/license.json:ro"
      ], 
    "instance-settings": {"autofill-username": "myname", "autofill-password": "mypassword"}
...
}
```
> **mounts**: add mounts for autofill settings and license.
> - the destination path for the JSON file with autofill settings in the projector instance **MUST** be  */opt/supra/conf/autofill.json*.<br>
> - the destination path for the license file in the projector instance **MUST** be  */opt/supra/conf/license.json*.<br>
>
> **instance-settings**: set the values for autofill fields defined in **fields** in the JSON file with autofill settings.<br>
>> The name for an autofill field **MUST** be "autofill-{fieldType}", and the value will overwrite the default value specified in the JSON file.<br> 
<br>
> NOTE: If the **fieldValue** for a field is blank in the JSON file AND neither is the value set in **instance-settings**, the autofill script will trigger a click on the field.


## Autofill 
Besides of autofill settings in the password for VNC connection, to enable autofill for a RBI session the user needs to have a JSON file with autofill settings on the host and properly setup the password for VNC connection.

### JSON file with autofill settings
The json file provides settings for autofill in the target webpage. 

```
{
  "url": "https://www.target.com/path/",
  "site": "https://*.target.com/*",
  "fields": [
    {
      "fieldType": "username",
      "fieldValue": "",
      "location": [
        {
          "id": "username"
        }
      ]
    },
    {
      "fieldType": "password",
      "fieldValue": "",
      "location": [
        {
          "attr": "ht",
          "attrValue": "input_pwdlogin_pwd",
          "tag": "input"
        }
      ]
    },
    {
      "fieldType": "submitButton",
      "fieldValue": "",
      "location": [
        {
          "classname": "fm-submit"
        }
      ]
    }
  ]
}
```
> **url**: set the target URL for the RBI session. The value **MUST** be the same as the **username for VNC connection**.<br>
> **site**: set the site with regex. In the RBI session, when a web page with URL matches the regex, the autofill script will be activated.<br>
> **fields**: define the list of fields in a web page for the autofill script.<br>   
>> **fieldType**: set the type name of the target field.<br>
>> **fieldValue**: set the value for the target field with the autofill script. <br>
>> **location**: set information that can be used to locate the target field in a web page.<br>

