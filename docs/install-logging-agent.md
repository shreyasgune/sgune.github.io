## Install the logging and monitoring agents in the Compute Engine instance
The data for Nginx requests is missing because without the logging and monitoring agents being installed, the best Google Cloud can do is black-box monitoring. If you want to see more details, then you need to enable white-box monitoring by installing the agents.

Use the Navigation menu to navigate to Compute Engine | VM instances.

Click the SSH link for the web-server-vm

Check to see if the logging agent is installed/running.

```
sudo service google-fluentd status
sudo service stackdriver-agent status
```
Not surprisingly, the services could not be found. Check to make sure you have the requisite scopes to perform logging and monitoring.
```
curl --silent --connect-timeout 1 -f -H "Metadata-Flavor: Google" \
http://169.254.169.254/computeMetadata/v1/instance/service-accounts/default/scopes
```
Note in the response the logging.write and monitoring.write scopes.

Download the script, add the monitoring agent repo to apt, and install the agent.
```
curl -sSO https://dl.google.com/cloudagents/add-monitoring-agent-repo.sh
sudo bash add-monitoring-agent-repo.sh
sudo apt-get update
sudo apt-get install stackdriver-agent
```
Start the monitoring agent.
```
sudo service stackdriver-agent start
```
Install the logging agent.
```
curl -sSO https://dl.google.com/cloudagents/install-logging-agent.sh
sudo bash install-logging-agent.sh
```
Retest the two agents again and verify they are both active. If you see any "can not take infinite value" warnings, ignore them.
```
sudo service google-fluentd status
sudo service stackdriver-agent status
```
To fully integrate the server, you enable the status information handler in Nginx by adding a configuration file to the Nginx configuration directory.
```
(cd /etc/nginx/conf.d/ && sudo curl -O https://raw.githubusercontent.com/Stackdriver/stackdriver-agent-service-configs/master/etc/nginx/conf.d/status.conf)
```
Reload the Nginx service.
```
sudo service nginx reload
```
Enable the Nginx monitoring plugin.
```
(cd /opt/stackdriver/collectd/etc/collectd.d/ && sudo curl -O https://raw.githubusercontent.com/Stackdriver/stackdriver-agent-service-configs/master/etc/collectd.d/nginx.conf)
```
Restart the monitoring agent.
```
sudo service stackdriver-agent restart
```