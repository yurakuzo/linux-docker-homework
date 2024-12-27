# Netcat Docker Setup  

This repository contains two Docker images based on Alpine, configured to demonstrate communication using Netcat scripts.  

## Files  
- `Dockerfile-server`: Builds the server container.  
- `Dockerfile-client`: Builds the client container.  
- `server.sh`: Netcat server script.  
- `client.sh`: Netcat client script.  

## Instructions  

### 1. Build Images  
```bash  
docker build -t netcat-server -f Dockerfile-server .  
docker build -t netcat-client -f Dockerfile-client .  
```

### 2. Create Network
```bash
docker network create netcat-network
```

### 3. Run Containers
- Start the server:
```bash
docker run -it --rm --name server-container --network netcat-network netcat-server
```
- Start the client:
```bash
docker run -it --rm --name client-container --network netcat-network netcat-client
```

### 4. Verify Connectivity
- Start container and listen to logs on another port in real time
```bash
docker start server-container
docker exec -it server-container nc -lv -p 8001
```
- Open another termninal session and exec netcat ping to server container
```bash
docker start client-container
docker exec -it client-container /bin/sh
#>echo "Hello again!" | nc server-container 8001
```
- We should see following logs on server container:
```logs
Listening on 0.0.0.0 8001
Connection received on client-container.netcat-network 57214
Hello again!
```

