#TODO: create a Bolt plan to automate this
## Prerequisites

* Install `git` and `bolt`
* Create the required directories
```bash
mkdir -p /root/metrics_import /opt/mft-automations/{err,}
```
* Clone operational dashboards and install requirements
```bash
cd /root/metrics_import
git clone https://github.com/puppetlabs/puppet_operational_dashboards.git
bolt module install --force
/opt/puppetlabs/bolt/bin/gem install --user-install toml-rb
```

* Save files/metrics_import.sh to `/root/metrics_import/puppet_operational_dashboards`

## Set up InfluxDB and Grafana
* Run the `provision_dashboard` plan
```bash
cd /root/metrics_import/puppet_operational_dashboards
bolt plan run puppet_operational_dashboards::provision_dashboard --targets localhost
```

## Set up GPG key and passphrase
* Save the Support GPG key from 1pass to a file `key.asc`
* Import it
```bash
gpg --import key.asc
```
* Save the key's passphrase to a file `/root/.support_gpg`

## Set up the service to import metrics from sup scripts
* Save `files/metrics_import.service` to `/etc/systemd/system/metrics_import.service`
* Save `files/metrics_import.timer` to `/etc/systemd/system/metrics_import.timer`
* Do a daemon-reload
```bash
systemctl daemon-reload
```
* Confirm the timer is running
```bash
systemctl status metrics_import.timer
```
