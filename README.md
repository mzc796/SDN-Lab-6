# SDN-Lab-6
Build IP-based shortest path routing based on L2swtich project.

## Virtual Machine Summary
Memory: >= 8GB

Storage: 50GB

CPU: 2 cores, AMD64 Architecture

Installation Disc: [ubuntu-22.04.4-desktop-amd64.iso](https://old-releases.ubuntu.com/releases/22.04/)

## References
[An Instant Virtual Network on your Laptop (or other PC)](https://mininet.org/)

[PICOS 4.4.3 Configuration G](https://pica8-fs.atlassian.net/wiki/spaces/PicOS443sp/overview?homepageId=10453009)

[OpenDaylight Flow Examples](https://docs.opendaylight.org/projects/openflowplugin/en/latest/users/flow-examples.html)

[L2switch User Guide](https://test-odl-docs.readthedocs.io/en/latest/user-guide/l2switch-user-guide.html)
## Preparation
1. Download the code:
   ```
   git clone https://github.com/mzc796/SDN-Lab-6.git
   ```
2. Build project:
   ```
   cd SDN-Lab-6/
   mvn clean install -DskipTests -Dcheckstyle.skip
   ```
3. Run OpenDaylight-ShortestPath
   ```
   cd distribution/karaf/target/assembly/bin/
   sudo ./karaf
   ```
4. Run Mininet. Open a new terminal:
   ```
   cd SDN-Lab-6/mn/
   sudo ./run_mn_tree.sh
   ```
   ```
   s1
   ├── s2
   │   ├── s3
   │   │   ├── h1
   │   │   └── h2
   │   └── s4
   │       ├── h3
   │       └── h4
   └── s5
       ├── s6
       │   ├── h5
       │   └── h6
       └── s7
           ├── h7
           └── h8
   ```
5. Check connection.

   (1) Observe topology.
   ```
   cd SDN-Lab-6/odl-scripts/
   mkdir data
   sudo ./req_topo.sh
   ```
   (2) Observe default flow entries. For example:
   ```
   cd SDN-Lab-6/mn/
   sudo ./dump_flows.sh s3
   ```
7. Test IP-based shortest path routing. 

   Question: ICMP, TCP, UDP, which types of packets can be forwarded automatically? 

   (1) Test ICMP

   In the mininet terminal:
   ```
   xterm h1 h2
   ```
   In the h1 terminal:
   ```
   ping 10.0.0.2
   ```
   In the h2 terminal:
   ```
   ping 10.0.0.1
   ```
   (2) Test TCP

   In the h2 terminal:
   ```
   python3 tcp_server.py
   ```
   In the h1 terminal:
   ```
   python3 tcp_client.py
   ```
   (3) Test UDP

   In the h2 terminal:
   ```
   python3 udp_receiver.py 2
   ```
   In the h1 terminal:
   ```
   python3 udp_sender.py 1 2
   ```
   (4) Check flow entries and explain what's observed and why.
   
   In the system terminal:
   ```
   sudo ./dump_flows s3
   ```
8. Find the bug

   There is a bug in the code. Please send UDP packets from different hosts to find the bug and correct it.
   Hint: You could test `h1` sends UDP packets to `h2`, and `h3` sends UDP packets to `h2`.
   
10. Write down the workflow of UDP/TCP/ICMP packets from hosts to the switch, controller, ..., until they arrive at the destination host. 
    
