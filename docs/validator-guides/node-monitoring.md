# <b>HOW TO VISUALIZE NODE METRICS</b>
---

This guide will teach you how to use Prometheus and Grafana to implement a basic validator monitoring. 
We can do this because every SwapDEX node exposes metrics such as the chain height, number of connected peers or the amount of memory used on a Prometheus metric endpoint. 

To visualize these metrics we will use <a href="https://prometheus.io/" target="_blank"> Prometheus</a> to collect the data and <a href="https://grafana.com/" target="_blank"> Grafana</a> to visualize them on a nice looking dashboard.

We will implement the monitoring in the following three steps:
1. Install Prometheus and Grafana
2. Configure Prometheus to scrape your SwapDEX node
3. Visualize Prometheus Metrics with Grafana

## **Architecture**
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

Lets inspect the unzipped file we downloaded by changing directories:
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

**Let's configure Prometheus**

Before Prometheus can be started, it needs some configuration. We will manage the configuration in a `.yml` file which we will now create:

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

**Let's Start Prometheus**

Before we start Prometheus let's make sure that our firewall is not blocking port 9090
```
ufw allow 9090
```

To test that Prometheus is set up properly we will execute a command to inspect the logs while Prometheus is running:
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

We can also quickly verify if our SwapDEX node gets scraped by Prometheus by visiting the `Status-> Targets` page.

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
    Prometheus should be running now, and we should be able to access its front-end again end by re-visiting `{==IP_ADDRESS==}:9090/`.


### **Let's install Grafana**

To visualize those metrics Prometheus collects we use Grafana. We run the following commands to install it:

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

Now we can access it by going to `http://`{==SERVER_IP_ADDRESS==}:3000/login`. The default user and password is admin/admin.

!!! note
    If you want to change the port on which Grafana runs (3000 is a popular port), edit the file `/usr/share/grafana/conf/defaults.ini` with a command like `sudo vim /usr/share/grafana/conf/defaults.ini` and change the `http_port` value to something else. Then restart grafana with `sudo systemctl restart grafana-server`.


![img](assets/grafana_01.png#center)

!!! success
    We successfully installed and configured Prometheus and Grafana at this point :heart:

### **Let's visualize the node metrics**

Now that everything is running we need to connect our Prometheus datasource to our Grafana server. We do this in the UI of Grafana by going to `Data Sources`

![img](assets/grafana_02.png#center)

![img](assets/grafana_03.png#center)

![img](assets/grafana_04.png#center)

The only thing we need to input is the URL that is https://localhost:9090 and then click Save & Test. If you see Data source is working, your connection is configured correctly.

![img](assets/grafana_05.png#center)

![img](assets/grafana_06.png#center)

!!! success
    We connected our Prometheus database with Grafana!

### **Let's use a Grafana Template Dashboard**

For this guide we will use a standard Dashboard developed by substrate to monitor our node, but you can at all times customize or create your own Dashboards. 

![img](assets/grafana_07.png#center)

![img](assets/grafana_08.png#center)

![img](assets/grafana_09.png#center)
