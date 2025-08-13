# 5G RFsim – Docker-powered OAI RF Simulator

A lightweight and fully containerized setup for running the OpenAirInterface 5G RF simulator.  
This project enables you to emulate a complete 5G network — including the Core, gNB, and NR-UE — entirely in Docker, without requiring any physical RF hardware.  
It’s ideal for development, testing, and learning about 5G protocols in a controlled and reproducible environment.

---

## System Requirements
- **OS**: Ubuntu 20.04  
- **Tools**: Docker, Docker Compose  
- **Note**: You must be logged into Docker before pulling images.

---

## Step-by-Step Guide

### 1) Clone the OAI repository
```bash
git clone https://gitlab.eurecom.fr/oai/openairinterface5g.git
cd openairinterface5g/
```

### 2) Pull the required Docker images
Retrieve the official OAI 5G RF simulator images from Docker Hub:
```bash
docker pull oaisoftwarealliance/oai-gnb:latest
docker pull oaisoftwarealliance/oai-nr-ue:latest
docker pull oaisoftwarealliance/oai-amf:latest
docker pull oaisoftwarealliance/oai-smf:latest
docker pull oaisoftwarealliance/oai-upf:latest
docker pull oaisoftwarealliance/oai-nrf:latest
```

### 3️) Start the 5G Core Network (RF simulation mode)
Navigate to the RF simulator docker-compose directory and start the core network containers:
```bash
cd ci-scripts/yaml_files/5g_rfsimulator
docker-compose up -d mysql oai-amf oai-smf oai-upf oai-ext-dn
```
Expected output:
```
Creating network "rfsim5g-oai-public-net" with driver "bridge"
Creating network "rfsim5g-oai-traffic_net-net" with driver "bridge"

Creating rfsim5g-mysql      ... done
Creating rfsim5g-oai-upf    ... done
Creating rfsim5g-oai-amf    ... done
Creating rfsim5g-oai-smf    ... done
Creating rfsim5g-oai-ext-dn ... done
```

### 4️) Start the gNB in RF simulator mode
```bash
docker-compose up -d oai-gnb
```
Check logs to confirm gNB startup:
```bash
docker logs -f oai-gnb
```

### 5️) Start the NR-UE in RF simulator mode
```bash
docker-compose up -d oai-nr-ue
```
Check logs to confirm UE registration:
```bash
docker logs -f oai-nr-ue
```

### 6️) Verify connectivity
- Ensure that the UE successfully registers to the gNB.
- Confirm that the gNB is connected to the Core (AMF).
- Test IP connectivity from the UE to the external data network:
```bash
docker exec -it oai-nr-ue ping -c 4 8.8.8.8
```

### 7️) Shut down the environment
When finished, stop and remove all containers:
```bash
docker-compose down
```
## Architecture Overview

```
   +-------------------+          +-------------------+          +-------------------+
   |   NR-UE (RFsim)   | <------> |   gNB (RFsim)     | <------> |  5G Core Network   |
   +-------------------+          +-------------------+          +-------------------+
                                                                  | AMF | SMF | UPF   |
                                                                  | NRF | MySQL | DN  |
                                                                  +-------------------+
```
