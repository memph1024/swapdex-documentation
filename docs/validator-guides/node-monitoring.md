# <b>HOW TO VISUALIZE NODE METRICS</b>
---

![img](assets/dashboard.png#center)

This guide will teach you how to use Prometheus and Grafana to implement a basic validator monitoring. 
We can do this because every SwapDEX node exposes metrics such as the chain height, number of connected peers or the amount of memory used on a Prometheus metric endpoint. 

We will use <a href="https://prometheus.io/" target="_blank"> Prometheus</a> to collect the data and <a href="https://grafana.com/" target="_blank"> Grafana</a> to visualize them on a nice looking dashboard.


## **Example Monitoring Architecture**
---

```
+--------------+                  +-------------+                                                              +---------+
| SwapDEX Node |                  | Prometheus  |                                                              | Grafana |
+--------------+                  +-------------+                                                              +---------+
      |               -----------------\ |                                                                          |
      |               | Every 1 minute |-|                                                                          |
      |               |----------------| |                                                                          |
      |                                  |                                                                          |
      |        GET current metric values |                                                                          |
      |<---------------------------------|                                                                          |
      |                                  |                                                                          |
      | `substrate_peers_count 5`        |                                                                          |
      |--------------------------------->|                                                                          |
      |                                  | --------------------------------------------------------------------\    |
      |                                  |-| Save metric value with corresponding time stamp in local database |    |
      |                                  | |-------------------------------------------------------------------|    |
      |                                  |                                         -------------------------------\ |
      |                                  |                                         | Every time user opens graphs |-|
      |                                  |                                         |------------------------------| |
      |                                  |                                                                          |
      |                                  |       GET values of metric `substrate_peers_count` from time-X to time-Y |
      |                                  |<-------------------------------------------------------------------------|
      |                                  |                                                                          |
      |                                  | `substrate_peers_count (1582023828, 5), (1582023847, 4) [...]`           |
      |                                  |------------------------------------------------------------------------->|
      |                                  |                                                                          |
```

## **Step 1 - Install Prometheus and Grafana**
---

To protect our node we will create a user for Prometheus but not allow Prometheus to log-in itself. 
We will then create the directories required to store the Prometheus config and executable file and then change the ownership of these directories to the Prometheus user so that only Prometheus can access them.

Create Prometheus User:
```
sudo useradd --no-create-home --shell /usr/sbin/nologin prometheus
```

Create the directories required to store the configuration and executable files:
```
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
```

Change the ownership of these directories to `prometheus`:
```
sudo chown -R prometheus:prometheus /etc/prometheus
sudo chown -R prometheus:prometheus /var/lib/prometheus
```

### ** Install and Configure Prometheus**
---
After setting up the environment, update your OS, and install the latest Prometheus. You can check the latest release by visiting their <a href="https://prometheus.io/download/" target="_blank"> downloads</a> page.

```
sudo apt-get update && apt-get upgrade
wget https://github.com/prometheus/prometheus/releases/download/v2.33.5/prometheus-2.33.5.linux-amd64.tar.gz
tar xfz prometheus-*.tar.gz
```

Let's inspect the unzipped file we downloaded by changing directories:
```
cd prometheus-2.33.5.linux-amd64
```

The following two binaries are in the directory:

- prometheus -> Prometheus main binary file
- promtool

The following two directories (which contain the web interface, configuration files examples and the license) are in the directory:

- consoles
- console_libraries

We want to copy the downloaded bin files and move it to our local folder for bin files `/usr/local/bin/`

```
sudo cp ./prometheus /usr/local/bin/
sudo cp ./promtool /usr/local/bin/
```

We also want to change the ownership of those file to our Prometheus user:

```
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
```

We also want to copy and move the `consoles` and `console_libraries` folders into our local folder `/etc/prometheus`

```
sudo cp -r ./consoles /etc/prometheus
sudo cp -r ./console_libraries /etc/prometheus
```

We again want to change the ownership to our Prometheus user:

```
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
```


!!! success
    Now we successfully moved all folders and binaries of the downloaded folder into the correct local folders and changed their ownership to our Prometheus user.

Now we can safely delete the downloaded folder:
```
cd .. && rm -rf prometheus*
```

### **Configure Prometheus**

Before Prometheus can be started, it needs some configuration. We will manage the configuration in a `.yml` file which we will create now:

```
sudo nano /etc/prometheus/prometheus.yml
```

The config file is divided into three parts:

1. `global`
2. `rule_files`
3. `scrape_configs`

- `scrape_interval` defines how often Prometheus scrapes targets, while evaluation_interval controls how often the software will evaluate rules.
- `rule files` block contains information of the location of any rules we want the Prometheus server to load.
- `scrape_configs` contains the information which resources Prometheus monitors.

Configure the file as following:

```
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  # - "first.rules"
  # - "second.rules"

scrape_configs:
  - job_name: "prometheus"
    scrape_interval: 5s
    static_configs:
      - targets: ["localhost:9090"]
  - job_name: "SwapDEX_Node"
    scrape_interval: 5s
    static_configs:
      - targets: ["localhost:9615"]

```

!!! note
    Prometheus can manage multiple jobs. We currently have two jobs (Prometheus and SwapDEX_Node) and both are listening on localhost ports. 
    You can also change the target address to an IP address of another node if you want to centralize all the data. 

With the above configuration file, the first exporter is the one that Prometheus exports to monitor itself. As we want to have more precise information about the state of the Prometheus server we reduced the scrape_interval to 5 seconds for this job. The parameters static_configs and targets determine where the exporters are running. The second exporter is capturing the data from our node, and the port by default is 9615.

Let' check the validity of the configuration file by running 

```
sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml
```

And let's change the ownership of the config file to our Prometheus user:

```
sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml
```

### **Start Prometheus**

Before we start Prometheus let's make sure that our firewall isn't blocking port 9090
```
ufw allow 9090
```

To test that Prometheus is set up properly we will execute a command to inspect the logs:
Remember, we need to run this command as the Prometheus user due to the ownership of the files:

```
sudo -u prometheus /usr/local/bin/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus/ --web.console.templates=/etc/prometheus/consoles --web.console.libraries=/etc/prometheus/console_libraries
```

We should see a message like this:

```
level=info ts=2021-04-16T19:02:20.167Z caller=main.go:380 msg="No time or size retention was set so using the default time retention" duration=15d
level=info ts=2021-04-16T19:02:20.167Z caller=main.go:418 msg="Starting Prometheus" version="(version=2.26.0, branch=HEAD, revision=3cafc58827d1ebd1a67749f88be4218f0bab3d8d)"
level=info ts=2021-04-16T19:02:20.167Z caller=main.go:423 build_context="(go=go1.16.2, user=root@a67cafebe6d0, date=20210331-11:56:23)"
level=info ts=2021-04-16T19:02:20.167Z caller=main.go:424 host_details="(Linux 5.4.0-42-generic #46-Ubuntu SMP Fri Jul 10 00:24:02 UTC 2020 x86_64 ubuntu2004 (none))"
level=info ts=2021-04-16T19:02:20.167Z caller=main.go:425 fd_limits="(soft=1024, hard=1048576)"
level=info ts=2021-04-16T19:02:20.167Z caller=main.go:426 vm_limits="(soft=unlimited, hard=unlimited)"
level=info ts=2021-04-16T19:02:20.169Z caller=web.go:540 component=web msg="Start listening for connections" address=0.0.0.0:9090
level=info ts=2021-04-16T19:02:20.170Z caller=main.go:795 msg="Starting TSDB ..."
level=info ts=2021-04-16T19:02:20.171Z caller=tls_config.go:191 component=web msg="TLS is disabled." http2=false
level=info ts=2021-04-16T19:02:20.174Z caller=head.go:696 component=tsdb msg="Replaying on-disk memory mappable chunks if any"
level=info ts=2021-04-16T19:02:20.175Z caller=head.go:710 component=tsdb msg="On-disk memory mappable chunks replay completed" duration=1.391446ms
level=info ts=2021-04-16T19:02:20.175Z caller=head.go:716 component=tsdb msg="Replaying WAL, this may take a while"
level=info ts=2021-04-16T19:02:20.178Z caller=head.go:768 component=tsdb msg="WAL segment loaded" segment=0 maxSegment=4
level=info ts=2021-04-16T19:02:20.193Z caller=head.go:768 component=tsdb msg="WAL segment loaded" segment=1 maxSegment=4
level=info ts=2021-04-16T19:02:20.221Z caller=head.go:768 component=tsdb msg="WAL segment loaded" segment=2 maxSegment=4
level=info ts=2021-04-16T19:02:20.224Z caller=head.go:768 component=tsdb msg="WAL segment loaded" segment=3 maxSegment=4
level=info ts=2021-04-16T19:02:20.229Z caller=head.go:768 component=tsdb msg="WAL segment loaded" segment=4 maxSegment=4
level=info ts=2021-04-16T19:02:20.229Z caller=head.go:773 component=tsdb msg="WAL replay completed" checkpoint_replay_duration=43.716µs wal_replay_duration=53.973285ms total_replay_duration=55.445308ms
level=info ts=2021-04-16T19:02:20.233Z caller=main.go:815 fs_type=EXT4_SUPER_MAGIC
level=info ts=2021-04-16T19:02:20.233Z caller=main.go:818 msg="TSDB started"
level=info ts=2021-04-16T19:02:20.233Z caller=main.go:944 msg="Loading configuration file" filename=/etc/prometheus/prometheus.yml
level=info ts=2021-04-16T19:02:20.234Z caller=main.go:975 msg="Completed loading of configuration file" filename=/etc/prometheus/prometheus.yml totalDuration=824.115µs remote_storage=3.131µs web_handler=401ns query_engine=1.056µs scrape=236.454µs scrape_sd=45.432µs notify=723ns notify_sd=2.61µs rules=956ns
level=info ts=2021-04-16T19:02:20.234Z caller=main.go:767 msg="Server is ready to receive web requests."

```

Now let's try to reach the graphical UI of our Prometheus server by opening the browser and opening the following URL:

```
http://{==SERVER_IP_ADDRESS==}:9090/graph
```

![img](assets/prometheus_01.png#center)

We can also quickly verify if our SwapDEX node gets scraped by visiting the `Status-> Targets` page.

![img](assets/prometheus_02.png#center)

If you can see it you can close the process on your VPS by hitting ++ctrl+c++

Now that everything is running we want to automatically start the server during the boot process, so we have to create a new `systemd` configuration file with the following config:

```
sudo nano /etc/systemd/system/prometheus.service
```

```
[Unit]
  Description=Prometheus Monitoring
  Wants=network-online.target
  After=network-online.target

[Service]
  User=prometheus
  Group=prometheus
  Type=simple
  ExecStart=/usr/local/bin/prometheus \
  --config.file /etc/prometheus/prometheus.yml \
  --storage.tsdb.path /var/lib/prometheus/ \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries
  ExecReload=/bin/kill -HUP $MAINPID

[Install]
  WantedBy=multi-user.target
```

After we saved the file, we want to reload `systemd` and enable the service so that it will be loaded automatically during the operating system's startup.

```
sudo systemctl daemon-reload && systemctl enable prometheus && systemctl start prometheus
```

!!! success
    Prometheus should be running now, and we should be able to access its front-end again by re-visiting `{==IP_ADDRESS==}:9090/`.

## **Step 2 - Install Node Exporter**

Now we will install Prometheus's Node Exporter module which exposes hardware metrics like CPU load, RAM and storage usage.

Create Node Exporter User
```
sudo useradd --no-create-home --shell /usr/sbin/nologin node_exporter
```
Create the directories required to store the configuration and executable files:

```
sudo mkdir /etc/node_exporter
sudo mkdir /var/lib/node_exporter
```

Change the ownership of those files to our `Node_Exporter` user

```
sudo chown -R node_exporter:node_exporter /etc/node_exporter
sudo chown -R prometheus:node_exporter /var/lib/node_exporter
```


Install the latest version of Node Exporter from <a href="https://prometheus.io/download/#node_exporter" target="_blank"> downloads</a> page:

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
tar xvfz node_exporter-*.*-amd64.tar.gz
```

Change directory into the downloaded file: 
```
cd node_exporter-*.*-amd64
```

Let's copy the binary `node_exporter` to our local binary folder `/usr/local/bin/`

```
sudo cp ./node_exporter /usr/local/bin/
```

Let's also change the ownership of the binary file:
```
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

Now we can delete the downloaded node_exporter folder:
```
cd .. && rm -rf node_exporter*
```

Since the `Node Exporter` will run on port 9100 we need to allow that port on our firewall

```
ufw allow 9100
```

Let's test-run the node_exporter:

```
cd /usr/local/bin
./node_exporter
```

We can now observe the exposed metrics in our browser at:

```
http://{==SERVER_IP_ADDRESS==}:9100/metrics

```

After we confirmed that the `node_exporter` is running we can exit the process on our VPS by hitting ++ctrl+c++

Now that everything is running we want to automatically start the `node_exporter` during the boot process, so we have to create a new `systemd` configuration file with the following config:

```
sudo nano /etc/systemd/system/node_exporter.service
```

```
[Unit]
  Description=Node Exporter Monitoring
  Wants=network-online.target
  After=network-online.target

[Service]
  User=node_exporter
  Group=node_exporter
  Type=simple
  ExecStart=/usr/local/bin/node_exporter
  ExecReload=/bin/kill -HUP $MAINPID

[Install]
  WantedBy=multi-user.target
```

After we saved the file, we want to reload `systemd` and enable the service so that it will be loaded automatically during the operating system's startup.

```
sudo systemctl daemon-reload && systemctl enable node_exporter && systemctl start node_exporter
```

Finally, we must tell our Prometheus server to scrape the metrics exposed by the `node_exporter`.
We do this by configuring the `prometheus.yml` file we created earlier.

```
sudo nano /etc/prometheus/prometheus.yml
```

```
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  # - "first.rules"
  # - "second.rules"

scrape_configs:
  - job_name: "prometheus"
    scrape_interval: 5s
    static_configs:
      - targets: ["localhost:9090"]
  - job_name: "SwapDEX_Node"
    scrape_interval: 5s
    static_configs:
      - targets: ["localhost:9615"]
  - job_name: "Node_Exporter"
    scrape_interval: 5s
    static_configs:
      - targets: ["localhost:9100"]
```

Now we need to reboot the VPS:

```
reboot
```

After the reboot we can revisit our Prometheus server at:

```
http://{==SERVER_IP_ADDRESS==}/targets
```
You should see the following in your browser:

![img](assets/node_exporter_01.png#center)


## **Step 3 - Install Alert Manager**

In this section, let's configure the Alert Manager that notifies you if problems occour on your server. Alerts can be sent in Slack, Email, Matrix, or others. In this guide, we will configure an outage email alerting using Gmail.

First, download the latest binary of Alert Manager and unzip it by running the command below:

```
wget https://github.com/prometheus/alertmanager/releases/download/v0.23.0/alertmanager-0.23.0.linux-amd64.tar.gz
tar xvfz alertmanager-*.*-amd64.tar.gz
mv alertmanager-*.*-amd64/alertmanager /usr/local/bin/
```
### **Gmail Setup**

To allow Alert Manager to email you, you will need to generate something called an app password in your Gmail account. For details, click <a href="https://support.google.com/accounts/answer/185833?hl=en" target="_blank"> here</a> to follow the whole setup.

!!! hint
    Copy or save your app password! We need it soon.

### **Alert Manager Configuration**

We will now create a new folder for the Alert Manager's config file:

```
mkdir /etc/alertmanager
```

Now need to create a new configuration file called `alertmanager.yml` under `/etc/alertmanager`

```
sudo nano /etc/alertmanager/alertmanager.yml
```
and specify the file as follows:

```
global:
 resolve_timeout: 1m

route:
 receiver: 'gmail-notifications'

receivers:
- name: 'gmail-notifications'
  email_configs:
  - to: {==YOUR_EMAIL==}
    from: {==YOUR_EMAIL==}
    smarthost: smtp.gmail.com:587
    auth_username: {==YOUR_EMAIL==}
    auth_identity: {==YOUR_EMAIL==}
    auth_password: {==YOUR_APP_PASSWORD==}
    send_resolved: true
```

!!! note
    With the above configuration, alerts will be sent using the email you set above. Remember to change {==YOUR_EMAIL==} to your email and paste the app password you just saved earlier to the `YOUR_APP_PASSWORD`.

Now we change the ownership to our Prometheus user:

```
sudo chown -R prometheus:prometheus /etc/alertmanager
```
Now let's open another port on our firewall for the Alert Manager:

```
ufw allow 9093
```

Next, we will create another `systemd` file to make sure that the alert manager will start every time the server reboots:

```
sudo nano /etc/systemd/system/alertmanager.service
```

```
[Unit]
Description=AlertManager Server Service
Wants=network-online.target
After=network-online.target

[Service]
User=root
Group=root
Type=simple
ExecStart=/usr/local/bin/alertmanager --config.file /etc/alertmanager/alertmanager.yml --web.external-url=http://{==SERVER_IP==}:9093 --cluster.advertise-address='0.0.0.0:9093'


[Install]
WantedBy=multi-user.target
```
!!! note
    SERVER_IP - Change it to your host IP address.

Finally, we can start the Alert Manager with the following command:
```
sudo systemctl daemon-reload && sudo systemctl enable alertmanager && sudo systemctl start alertmanager && sudo systemctl status alertmanager
```

!!! note
    You should see: Active: active (running)
```
Created symlink /etc/systemd/system/multi-user.target.wants/alertmanager.service → /etc/systemd/system/alertmanager.service.
● alertmanager.service - AlertManager Server Service
     Loaded: loaded (/etc/systemd/system/alertmanager.service; enabled; vendor preset: enabled)
     {==Active: active (running)==} since Sun 2022-03-13 22:17:12 UTC; 12ms ago
   Main PID: 2484 (alertmanager)
      Tasks: 4 (limit: 9457)
     Memory: 948.0K
        CPU: 10ms
```

!!! tip
    For now that's it, but we will need to install a Grafana Plug-In later in this process but let's first install Grafana.
    Hang on buddy we are done soon :rocket:

## **Step 4 - Install Grafana**

To visualize those metrics we will use Grafana. We run the following commands to install it:

```
sudo apt-get install -y adduser libfontconfig1
wget https://dl.grafana.com/oss/release/grafana_8.4.3_amd64.deb
sudo dpkg -i grafana_8.4.3_amd64.deb
```

Now we will configure Grafana to autostart as well

```
sudo systemctl daemon-reload
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```

```
ufw allow 3000
```

Before we start to wire everything together, let's take care of the aforementioned alert manager plugin. It will help you to monitor the alert information. To install it, execute the command below:

```
sudo grafana-cli plugins install camptocamp-prometheus-alertmanager-datasource
```

and restart Grafana:

```
sudo systemctl restart grafana-server
```

Now we can access Grafana by going to `http://`{==SERVER_IP_ADDRESS==}:3000/login`. The default user and password is admin/admin.

!!! note
    If you want to change the port on which Grafana runs (3000 is a popular port), edit the file `/usr/share/grafana/conf/defaults.ini` with a command like `sudo vim /usr/share/grafana/conf/defaults.ini` and change the `http_port` value to something else. Then restart grafana with `sudo systemctl restart grafana-server`.


![img](assets/grafana_01.png#center)

!!! success
    We successfully installed and configured Prometheus, Node Exporter, Alert Manager and Grafana at this point :heart:

### **Let's us wire it all together**

Now that everything is running we need to connect our Prometheus and alert manager data source to our Grafana server. We do this in the UI of Grafana by going to `Data Sources`

![img](assets/grafana_02.png#center)

![img](assets/grafana_03.png#center)

![img](assets/grafana_04.png#center)

The only thing we need to input is the URL that is `https://localhost:9090` and then click Save & Test. If you see Data source is working, your connection is configured correctly.

![img](assets/grafana_05.png#center)

![img](assets/grafana_06.png#center)

!!! success
    We connected our Prometheus database with Grafana!

Now let's wire the Alert Manager similarly:

![img](assets/grafana_10.png#center)

![img](assets/grafana_11.png#center)

### **Let's use our SwapDEX Template Dashboard**

For this guide we will use a customized SwapDEX Dashboard to monitor our node, but you can always start to customize or create your own Dashboards. 

![img](assets/grafana_07.png#center)

![img](assets/grafana_08.png#center)

!!! tip
    You can get the import code here: <a href="https://grafana.com/grafana/dashboards/15891" target="_blank"> Grafana Labs</a>.  May also consider to give it a review :smile:

![img](assets/grafana_09.png#center)

!!! success
    There you go mate :fire: :rocket:


<br></br>

<p align=right> Written by Petar </p>