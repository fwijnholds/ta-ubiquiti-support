# Installing and configuring Ubiquiti add-on for Splunk

To use this TA with Enterprise Security some configuration is required. Datamodel acceleration at scale requires from the deployment that data is assigned to indexes by it's cardinality. As Sourcetype is assigned by input in Splunk there are 2 ways achieving this split. One is using transforms.conf in conjuction with a props.conf, the other is by having an intermediate like a syslog server do the splitting into files. A UF can assign sourcetype input.

Which method you chose (even non documented ones) make sure the input is assigned the ubqt sourcetype! 

[udp://8514]
connection_host = ip 
sourcetype = ubnt 
index=ubnt

All transforms will look for this sourcetype, the TA won't work as expected if you name the source differently. 

## Method 1:

If you want to use this add-on on a single instance, with full CIM compatability follow these steps:

Download transforms.conf-single and rename to transforms.conf. 
https://github.com/fwijnholds/ta-ubiquiti-support/blob/master/transforms.conf-single.conf

Download props.conf-single and rename to props.conf.
https://github.com/fwijnholds/ta-ubiquiti-support/blob/master/props.conf-single

This file holds the configuration which rewrites the sourcetype based on a patern match, and routes the traffic to the correct index.
Place transforms.conf in the ta-ubiquiti/local directory.
Place props.conf in the ta-ubiquiti/local directory.

Download the ubqt_indexes.spl and install this on your indexers. 
https://github.com/fwijnholds/ta-ubiquiti-support/blob/master/org_all_indexes.spl

note: This contains a standard set of indexes, the Splunk best practice for CIM you will note that it separates OS logs from Network logs and Security logs from Application logs. The idea here is to separate them for performance reasons, but also for isolation purposes-you may want to expose the application or system logs to people who shouldn't view security logs. Putting them in separate indexes prevents that.


Restart splunk

In the UI navigate to Settings -> Data Inputs
Add a new input for UDP
Enter the port which you would like to receive the data on
Select any ubqt sourcetype ie ubqt:fw, change settings to your liking with the exception of indexes. Or manually create inputs.conf

[udp://8514]
connection_host = ip 
sourcetype = ubnt 
index=ubnt

## Method 2:

For deployment with a syslog server the recomendation is to use a Universal forwarder on your rsyslog or syslog-ng server. Have the syslog server write a file per sourcetype and have the UF assign the sourcetype to the specific logs. More information will follow.
