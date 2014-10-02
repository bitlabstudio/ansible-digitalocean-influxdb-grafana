# Ansible Playbook for InfluxDB & Grafana on DigitalOcean

This playbook aims to install [InfluxDB](http://influxdb.com/) and
[Grafana](http://grafana.org/) on a Ubuntu 14.10 droplet on
[DigitalOcean](https://www.digitalocean.com/?refcode=eea42e4b1499).

Grafana will be served via [nginx](http://nginx.org/), InfluxDB will be
accessible throught it's default webserver.

I haven't figured out how to run this stack with an SSL cert, so this is rather
insecure at the moment. For some reason, InfluxDB doesn't seem to work with
the same self signed SSL cert that I created for nginx.

# Usage

First setup your droplet on DigitalOcean. Make sure to upload your public
RSA key and select that key when creating your droplet. This will allow you to
login as root without entering a password.

Make sure that you have Ansible installed. I wouldn't even bother to create a
virtualenv for this because it is quite unlikely that you will ever need
several different versions of Ansible on your machine. The latest should always
do.

```
sudo pip install ansible --upgrade
```

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

Execute the playbook:

```
ansible-playbook -i hosts site.yml
```

Wasn't that easy? ;)
