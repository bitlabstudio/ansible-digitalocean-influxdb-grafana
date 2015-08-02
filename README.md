# Ansible Playbook for InfluxDB & Grafana on DigitalOcean

This playbook aims to install [InfluxDB](http://influxdb.com/) and
[Grafana](http://grafana.org/) on a Ubuntu 14.10 droplet on
[DigitalOcean](https://www.digitalocean.com/?refcode=eea42e4b1499).

The whole process took me 4 minutes from droplet creation to finished
installation.

# Quick quide

```
apt-get install python-devel python-pip
sudo pip install ansible --upgrade
git clone https://github.com/bitmazk/ansible-digitalocean-influxdb-grafana.git
cd ansible-digitalocean-influxdb-grafana
cp hosts.sample hosts
vim hosts
cp vars/external_vars.yml.sample vars/external_vars.yml
vim vars/external_vars.yml
ansible-playbook -i hosts site.yml
```

# Detailed instructions

Grafana will be served via [nginx](http://nginx.org/), InfluxDB will be
accessible throught it's default webserver.

I haven't figured out how to run this stack with an SSL cert, so this is rather
insecure at the moment. For some reason, InfluxDB doesn't seem to work with
the same self signed SSL cert that I created for nginx.

## Create droplet & upload SSH key

First setup your droplet on DigitalOcean. Make sure to upload your public
SSH key and select that key when creating your droplet. This will allow you to
login as root without entering a password.

## Install Ansible

Make sure that you have Ansible installed. I wouldn't even bother to create a
virtualenv for this because it is quite unlikely that you will ever need
several different versions of Ansible on your machine. The latest should always
do.

```
sudo pip install ansible --upgrade
```

## Clone this repository & change variables

Now clone this repository and ``cd`` into the cloned folder.

Copy the ``hosts.example`` file and enter the IP address of your DigitalOcean
droplet.

```
cp hosts.example hosts
vim hosts
```

Copy the ``external_vars.yml.example`` file and change all usernames and
passwords to your liking.

```
cp vars/external_vars.yml.sample vars/external_vars.yml
vim vars/external_vars.yml
```

## Run the playbook

Execute the playbook:

```
ansible-playbook -i hosts site.yml
```

Wasn't that easy? ;)

## Create awesome dashboards

You can now visit ``http://<droplet-ip>:8083`` and see your InfluxDB admin.
You will see that we have two databases, one for saving grafana dashboards and
another for your project's metrics.

You can also visit ``http://<droplet-ip>`` to see your grafana dashboard.

## Update Grafana

When a new version of Grafana is released, just change the
``GRAFANA_SOURCE_FILENAME`` in ``external_vars.yml`` and run:

```
ansible-playbook -i hosts update-grafana.yml
```
