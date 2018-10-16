# ta-ubiquiti-support

If you want to use this add-on on a single instance, with full CIM compatability follow these steps:

- Download transforms.conf-single and rename to transforms.conf.
   This file holds the configuration which rewrites the sourcetype based on a patern match, and routes the traffic to the correct index.
   
- Place transforms.conf in the ta-ubiquiti/local director.
- Add following line to ta-ubiquiti/local/props.conf

[default]
TRANSFORMS-changesourcetypes = sourcetype-hostapd, sourcetype-mcad, index-mcad, sourcetype-sudo, index-mcad, sourcetype-dhcp, index-dhcp, sourcetype-fw, index-fw, sourcetype-threat, index-threat

- Download and install the ubqt_indexes.tar
- Restart splunk
- In the UI navigate to Settings -> Data Inputs
- Ad a new input for UDP 
- Enter the port which you would like to receive the data on
- Select any ubqt sourcetype ie ubqt:fw, change settings to your liking with the exception of indexes. Or manually create inputs.conf 

[udp://8514]

connection_host = ip
sourcetype = ubqt
index=ubqt

For deployment with a syslog server the recomendation is to use a Universal forwarder on your rsyslog or syslog-ng server. Have the syslog server
write a file per sourcetype and have the UF assign the sourcetype to the specific logs. More information will follow.
