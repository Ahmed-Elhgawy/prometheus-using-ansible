# Prometheus Installation using ansible

<img src="img/Grafana.png" alt="drawing" width="100" height="100"/><img src="img/Prometheus.png" alt="drawing" width="100" height="100"/> 


## 1- Prometheus

-Install prometheus v2.43.0 as a service on ubuntu using ansible

-Prometheus works at port **9090**

## 2- Node Exporter

-Install node exporter v1.3.1 as a service on same instance where is prometheus

-Node Exporter works at port **9100**

-Don't forget to add node exporter job name to prometheus configration file "/etc/prometheus/prometeus.yml" and restart prometheus service `sudo systemctl restart prometheus.service`

-Example:

you can change "localhost:9100" with the ip of instance where node exporter is installed and if you have more than one instance you can add more than one job

```
- job_name: "nodeExporterJob"
  static_configs:
    - targets: ["localhost:9100"]
```

## 3- cAdvisor

-cAdvisor will run as a container using docker and works at port **8080**

```
sudo docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:ro \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  --privileged \
  --device=/dev/kmsg \
  gcr.io/cadvisor/cadvisor
```

-Don't forget to add cAdvisor job name to prometheus configration file "/etc/prometheus/prometeus.yml" and restart prometheus service `sudo systemctl restart prometheus.service`

-Example:

you can change "localhost:8080" with the ip of instance where cAdvisor is installed ans if you have more than one instance you can add more than one job

```
- job_name: "cadvisorJob"
  static_configs:
    - targets: ["localhost:8080"]
```

## 4- Alert Manager 

-Install alert manager v0.22.2 as a service on same instance where is promethues

-Alert manager work at port **9093**

-Don't forget to add alert manager job name to prometheus configration file "/etc/prometheus/prometeus.yml" and restart prometheus service `sudo systemctl restart prometheus.service`

-Example:

you can change "localhost:9093" with the ip of instance where alert manager is installed and if you have more than one instance you can add more than one job

```
- job_name: "alertManagerJob"
  static_configs:
    - targets: ["localhost:9093"]
```

## 5- Grafana

-Install grafana on the same instance where is prometheus

-Grafana works at port **3000**

### CLient library 

-client library is a nodejs app to test rules and alert manger comment its role in ansible playbook (site.yml) if you don't need it

### **Don't Forget to add your instance ip in inventory file**
