# PVENoSubWarning
A quick little guide for Proxmox Virtual Environment 8.3.2 and Proxmox Backup Server 3.3.2.


## Backup Original
```bash
sudo cp /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js /root/proxmoxlib.js.bak
```
## Restore Original
```bash
sudo cp /root/proxmoxlib.js.bak /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
```
# Disable NoSub Popup

## Remove lines 554-585
```bash
sudo sed -i '554,585d' /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
```
## Insert the new block at line 554 and include a blank line
```bash
sudo sed -i '554i\
    checked_command: function(orig_cmd) {\
        Proxmox.Utils.API2Request(\
            {\
                url: "/nodes/localhost/subscription",\
                method: "GET",\
                failure: function(response, opts) {\
                    Ext.Msg.alert(gettext("Error"), response.htmlStatus);\
                },\
                success: function(response, opts) {\
                    let res = response.result;\
                    if (res === null || res === undefined || !res || res\
                        .data.status.toLowerCase() !== "active") {\
                        //Ext.Msg.show({\
                            //title: gettext("No valid subscription"),\
                            //icon: Ext.Msg.WARNING,\
                            //message: Proxmox.Utils.getNoSubKeyHtml(res.data.url),\
                            //buttons: Ext.Msg.OK,\
                            //callback: function(btn) {\
                                //if (btn !== "ok") {\
                                    //return;\
                                //}\
                                //orig_cmd();\
                            //},\
                        //});\
                        orig_cmd();\
                    } else {\
                        orig_cmd();\
                    }\
                },\
            },\
        );\
    },\
\' /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
```
# Disable Repository Warnings

## Remove lines 19795-19802
```bash
sudo sed -i '19795,19802d' /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
```
## Insert the new block at line 19794
```bash
sudo sed -i '19794i\
                if (repos.nosubscription) {\
                    //addWarn(Ext.String.format(\
                        //gettext("The {0}no-subscription{1} repository is not recommended for production use!"),\
                        //type,\
                        //noSubAlternateName,\
                    //));\
                }' /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
```
