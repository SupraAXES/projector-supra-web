# SupraRBI-VNC and Apache Guacamole

A good solution for secure access to a web page/site or application is **projector-supra-web** together with [SupraRBI-VNC](https://github.com/supraaxes/suprarbi-vnc) and [Apache Guacamole](https://guacamole.apache.org/).

No modification to Apache Guacamole client *guacmaole/guacamole* or server *guacamole/guacd* is required, and all sessions to the target URL can be managed just as any other VNC session.

## SupraRBI-VNC
Set *SUPRA_PROJECTOR_NETWORK* and *SUPRA_PROJECTOR_IMAGE* for **SupraRBI-VNC** server.

```
# Please ensure the guacd container is in the same network as the SupraRBI-VNC server, *supra-projector*.

docker run --name vnc-rbi -d \
    --network supra-projector \
    -p 5900:5900 \
    -e SUPRA_PROJECTOR_NETWORK='supra-projector' \
    -e SUPRA_PROJECTOR_IMAGE='supraaxes/projector-supra-web' \
    -v /var/run/docker.sock:/var/run/docker.sock \
    supraaxes/suprarbi-vnc
```

## Guacamole Configuration Examples

The configuration on Apache Guacamole is straight forward as any other VNC connection, and for detailed information about session settings in *password*, please check [SupraRBI-VNC](https://github.com/supraaxes/suprarbi-vnc/README.md)

### Dynamic session to https://github.com, without license file

> Name: *rbi-github*<br>
> Protocol: *VNC*<br>
> Hostname: *vnc-rbi*<br>
> Port: *5900*<br>
> Username: *https://github.com*<br>
> Password: *{}*<br>

### Shared session to https://www.google.com, with audio on, with license file

> *NOTE:*<br> 
>   *1. A shared session is for all users;* <br>
>   *2. The license file on host is /opt/supra/rbi/conf/license.json*<br>
><br>
> Name: *rbi-google*<br>
> Protocol: *VNC*<br>
> Hostname: *vnc-rbi*<br>
> Port: *5900*<br>
> Username: *https://www.google.com*<br>
> Password: *{"id":{"type":"norm","name":"1234"},"mounts":["/opt/supra/rbi/conf/license.json:/opt/supra/conf/license.jsob:ro"]}*<br>
> Enable audio: *checked*<br>
> Audio server name: *rbi-norm-1234*<br>

### Dynamic session to https://github.com, with auto login, with license file. 
    
> *NOTE:*<br> 
>   *1. The guacd container is in the same network as the SupraRBI-VNC server;* <br>
>   *2. The github autofill setting file on host is /opt/supra/rbi/conf/autofill/github.json;* <br>
>   *3. The license file on host is /opt/supra/rbi/conf/license.json*<br>
><br>
> Name: *rbi-github*<br>
> Protocol: *VNC*<br>
> Hostname: *vnc-rbi*<br>
> Port: *5900*<br>
> Username: *https://github.com*<br>
> Password: *{"mounts":["/opt/supra/rbi/conf/autofill/github.json:/opt/supra/conf/autofill.json:ro","/opt/supra/rbi/conf/license.json:/opt/supra/conf/license.jsob:ro"],"instance-settings":{"autofill-username":"myname","autofill-password":"mypassword"}}*<br>