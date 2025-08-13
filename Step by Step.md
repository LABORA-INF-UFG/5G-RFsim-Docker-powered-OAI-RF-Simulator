System: Ubuntu 20.04

## Step-by-Step Guide
You need docker to be logged into your account to proceed.

```
git clone https://gitlab.eurecom.fr/oai/openairinterface5g.git
cd openairinterface5g/
```
### 1) Pull the required Docker images
Retrieve the official OAI 5G RF simulator images from Docker Hub:
```bash
docker pull oaisoftwarealliance/oai-gnb:latest
docker pull oaisoftwarealliance/oai-nr-ue:latest
docker pull oaisoftwarealliance/oai-amf:latest
docker pull oaisoftwarealliance/oai-smf:latest
docker pull oaisoftwarealliance/oai-upf:latest
docker pull oaisoftwarealliance/oai-nrf:latest

```
```
cd ci-scripts/yaml_files/5g_rfsimulator
docker-compose up -d mysql oai-amf oai-smf oai-upf oai-ext-dn
Creating network "rfsim5g-oai-public-net" with driver "bridge"
Creating network "rfsim5g-oai-traffic_net-net" with driver "bridge"
```
Creating rfsim5g-mysql      ... done
Creating rfsim5g-oai-upf   ... done
Creating rfsim5g-oai-amf   ... done
Creating rfsim5g-oai-smf   ... done
Creating rfsim5g-oai-ext-dn ... done
```
