# **GOOGLE VPC**

A VPC network (Google Virtual Private Cloud network) is very similar to a physical network, except that it is virtualized within the Google Cloud Platform (GCP).

A VPC network is a global resource that consists of a list of regional virtual subnetworks (subnets) in data centers, all connected by a global wide area network. VPC networks are logically isolated from each other in the Google Cloud Platform.

From the above information, we have understood that it is a network that is virtualized within the Google Cloud. So there may be network vulnerabilities or misconfigurations through which malicious activities could occur in our cloud network.

### **&#x20;**1 )  Check for Legacy Networks****

&#x20; Legacy network is no longer recommended for production environments because it does not support advanced networking features.

To determine if legacy networks are being used within your Google Cloud Platform (GCP) projects, perform the following operations:

1. So first list all the project from your google cloud .we will use the following command

```
  gcloud projects list --format="table(projectId)"
```

In the above command, we can list the projects exists in gcloud, and the --format command is used to show the projects in a specific manner.

2. The command output should return the requested GCP project identifiers (IDs) shown below:

```
PROJECT_ID
cc-project5-stack-123456
cc-production-app-123123

```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the selected project example shown below:

```
NAME                                 SUBNET_MODE
cc-web-stack-network                   LEGACY
cc-custom-vpc-network                  CUSTOM

```

5. &#x20;If the compute networks list command output lists LEGACY as the SUBNET\_MODE , as shown in the example above, the cloud legacy network is being used within the selected Google Cloud Platform project.

### **&#x20;2 ) Check for Unrestricted DNS Access**

Google Cloud VPC network firewall rules do not allow unrestricted access (i.e. 0.0.0.0/0) on TCP and UDP port 53 in order to reduce the attack surface and protect the virtual machine instances.

To determine if your Google Cloud VPC firewall rules allow unrestricted access on TCP and/or UDP port 53, perform the following actions:

1. So first list all the projects from your google cloud .we will use the following command

```
  gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-web-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. _--filter network_ command is used for filtering the network and  _sort_ command is used for sorting the priority

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The above command will give following output:

```
NAME                      DISABLED     DIRECTION          SOURCE_RANGES             ALLOW
cc-web-allow-dns           False       INGRESS              ['0.0.0.0/0']           tcp:53
cc-web-allow-icmp          False       INGRESS              ['0.0.0.0/0']           icmp
cc-web-allow-ssh           False       INGRESS              ['0.0.0.0/0']           tcp:22

```

From the above output, we can see port number 53 which is not disabled.

### **3 ) Check for Unrestricted FTP Access**

Keeping port 21 (FTP) can allow brute-force attacks, FTP bounce attacks, spoofing, and packet capture attacks.

To determine if your Google Cloud VPC firewall rules allow unrestricted access on TCP ports 20 and 21, perform the following operations:

1. So first list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-web-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. --filter network command is used for filtering the network and  sort command is used for sorting the priority.

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                   DISABLED   DIRECTION   SOURCE_RANGES     ALLOW
cc-web-allow-http      False      INGRESS     ['0.0.0.0/0']     tcp:80
cc-web-allow-https     False      INGRESS     ['0.0.0.0/0']     tcp:443
cc-web-allow-ftp       False      INGRESS     ['0.0.0.0/0']     tcp:21
```

From above output we can see port number 21 in not disabled.

### **&#x20;4 ) Check for Unrestricted ICMP Access**

Allowing unrestricted inbound/ingress ICMP access using VPC network firewall rules can increase opportunities for malicious activities such as Denial-of-Service (DoS) attacks, Smurf and Fraggle attacks.

To determine if your Google Cloud VPC firewall rules allow unrestricted ICMP access, perform the following operations:

1. So first list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-mobile-project-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-web-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. --filter network command is used for filtering the network and  sort command is used for sorting the priority.

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                   DISABLED   DIRECTION   SOURCE_RANGES     ALLOW
cc-app-allow-icmp      False      INGRESS     ['0.0.0.0/0']     icmp 
cc-app-allow-http      False      INGRESS     ['0.0.0.0/0']     tcp:80
cc-app-allow-https     False      INGRESS     ['0.0.0.0/0']     tcp:443
```

From above example output we can see imcp port is not disabled.

### **5 ) Check for Unrestricted Inbound Access on Uncommon Ports**

Allowing unrestricted (0.0.0.0/0) inbound access to uncommon ports via VPC network firewall rules can increase opportunities for malicious activities such as hacking, data capture, and all kinds of attacks (brute-force attacks, man-in-the-middle attack, Denial-of-Service attacks, etc).

To determine if your Google Cloud VPC firewall rules allow unrestricted ingress access to uncommon TCP/UDP ports, perform the following operations:

1. First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-dev-project-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-app-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. --filter network command is used for filtering the network and  sort command is used for sorting the priority.

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                         DISABLED  DIRECTION  SOURCE_RANGES  ALLOW
cc-allow-ica-access          False     INGRESS    ['0.0.0.0/0']  tcp:1494
cc-allow-ws-console-access   False     INGRESS    ['0.0.0.0/0']  tcp:8010
cc-allow-ws-database-access  False     INGRESS    ['0.0.0.0/0']  tcp:1433
cc-allow-http-access         False     INGRESS    ['0.0.0.0/0']  tcp:80
```

Check the compute firewall-rules list command output for any enabled firewall rules (i.e. DISABLED attribute set to False) with the DIRECTION set to INGRESS, SOURCE\_RANGES set to \['0.0.0.0/0'], and ALLOW set to an uncommon TCP/UDP port (e.g. TCP 1494). If one or more rules match the search criteria, there are VPC network firewall rules that allow unrestricted inbound/ingress access to uncommon ports, therefore the access to the associated virtual machine (VM) instances is not secured (restricted).

### **6 ) Check for Unrestricted MySQL Database Access**

Allowing unrestricted ingress access on TCP port 3306 (MySQL Database Server) through VPC network firewall rules can increase opportunities for malicious activities such as brute-force or bypass authentication attacks, and SQL injection attacks.

To determine if your Google Cloud VPC firewall rules allow unrestricted access on TCP port 3306, perform the following actions:

1. First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-web-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. --filter network command is used for filtering the network and  sort command is used for sorting the priority.

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                   DISABLED   DIRECTION   SOURCE_RANGES     ALLOW
cc-web-allow-mysql     False      INGRESS     ['0.0.0.0/0']     tcp:3306
cc-web-allow-https     False      INGRESS     ['0.0.0.0/0']     tcp:443
```

Check the compute firewall-rules list command output for any active firewall rules  with the DIRECTION set to INGRESS, SOURCE\_RANGES set to \['0.0.0.0/0'], and ALLOW set to tcp:3306 or tcp:0-65535. If one or more rules match the search criteria, there are VPC network firewall rules that allow unrestricted access on TCP port 3306, therefore the MySQL database access to the associated Google Cloud VM instances is not restricted/secured.

### **7 ) Check for Unrestricted Oracle Database Access**

Allowing unrestricted ingress/inbound access on TCP port 1521 through VPC network firewall rules can increase opportunities for malicious activities such as denial-of-service attacks, brute-force and man-in-the-middle (MITM) attacks, and can ultimately lead to data loss. VPC firewall rules should be configured so that access to specific resources is restricted to just those hosts or networks that have a legitimate business requirement for access.

To determine if your Google Cloud VPC firewall rules allow unrestricted access on TCP port 1521, perform the following actions:

1. First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-web-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. --filter network command is used for filtering the network and  sort command is used for sorting the priority.

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                   DISABLED   DIRECTION   SOURCE_RANGES     ALLOW
cc-web-allow-http      False      INGRESS     ['0.0.0.0/0']     tcp:80
cc-web-allow-https     False      INGRESS     ['0.0.0.0/0']     tcp:443
cc-web-allow-oracle    False      INGRESS     ['0.0.0.0/0']     tcp:1521
```

Check the compute firewall-rules list command output for any active firewall rules with the DIRECTION set to INGRESS, SOURCE\_RANGES set to \['0.0.0.0/0'], and ALLOW set to tcp:3306 or tcp:0-65535. If one or more rules match the search criteria, there are VPC network firewall rules that allow unrestricted access on TCP port 3306, therefore the MySQL database access to the associated Google Cloud VM instances is not restricted/secured.

### **8 ) Check for Unrestricted Outbound Access on All Ports**

Allowing unrestricted outbound/egress access on all TCP/UDP ports can increase opportunities for malicious activities such as Distributed Denial of Service (DDoS) attacks.

To determine if your Google Cloud VPC firewall rules allow unrestricted outbound access on all ports, perform the following actions:

1. &#x20;First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-web-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. _--filter network_ command is used for filtering the network and  _sort_ command is used for sorting the priority

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                   DISABLED  DIRECTION  DESTINATION_RANGES  ALLOW
cc-allow-http-access   False     EGRESS     ['0.0.0.0/0']       tcp:80
cc-allow-https-access  False     EGRESS     ['0.0.0.0/0']       tcp:443
cc-allow-wide-access   False     EGRESS     ['0.0.0.0/0']       tcp:0-65535
```

Check the compute firewall-rules list command output for any active firewall rules  with the DIRECTION set to EGRESS, DESTINATION\_RANGES set to \['0.0.0.0/0'], and ALLOW set to tcp:0-65535 or udp:0-65535. If one or more rules match the search criteria, there are VPC network firewall rules that allow unrestricted outbound traffic on any TCP/UDP port from the associated Google Cloud resources.

### **9 )  Check for Unrestricted PostgreSQL Database Access**

Allowing unrestricted inbound access on TCP port 5432 (PostgreSQL Database) via VPC network firewall rules can increase opportunities for malicious activities such as hacking, brute-force attacks, DDoS, and SQL injection attacks.

To determine if your Google Cloud VPC firewall rules allow unrestricted access on TCP port 5432, perform the following operations:

1. &#x20;First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-web-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. _--filter network_ command is used for filtering the network and  _sort_ command is used for sorting the priority

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                   DISABLED   DIRECTION   SOURCE_RANGES     ALLOW
cc-app-allow-http      False      INGRESS     ['0.0.0.0/0']     tcp:80
cc-app-allow-https     False      INGRESS     ['0.0.0.0/0']     tcp:443
cc-app-allow-postgres  False      INGRESS     ['0.0.0.0/0']     tcp:5432
```

Check the compute firewall-rules list command output for any enabled firewall rules  with the DIRECTION set to INGRESS, SOURCE\_RANGES set to \['0.0.0.0/0'], and ALLOW set to tcp:5432 or tcp:0-65535. If one or more rules match the search criteria, there are VPC network firewall rules that allow unrestricted access on TCP port 5432, therefore the PostgreSQL-based access to the associated Google Cloud resources is not restricted/secured.

### **10 ) Check for Unrestricted RDP Access**

Allowing unrestricted Remote Desktop Protocol (RDP) access can increase opportunities for malicious activities such as hacking, Man-In-The-Middle attacks (MITM) and Pass-The-Hash (PTH) attacks.

To determine if your VPC firewall rules allow unrestricted access on TCP port 3389 (RDP), perform the following actions:

1. &#x20;First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-web-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. _--filter network_ command is used for filtering the network and  _sort_ command is used for sorting the priority

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                   DISABLED  DIRECTION  SOURCE_RANGES     ALLOW
cc-app-allow-rdp       False     INGRESS    ['0.0.0.0/0']     tcp:3389
cc-app-allow-internal  False     INGRESS    ['10.105.0.0/9']  tcp:0-65535,udp:0-65535
```

Check the compute firewall-rules list command output for any enabled firewall rules with the DIRECTION set to INGRESS, SOURCE\_RANGES set to \['0.0.0.0/0'], and ALLOW set to tcp:3389 or tcp:0-65535. If one or more rules match the search criteria, there are VPC firewall rules that allow unrestricted access on TCP port 3389, defined for the selected VPC, therefore the RDP access to the associated Google Cloud Windows instances is not secured.

### **11 ) Check for Unrestricted RPC Access**

Allowing unrestricted ingress/inbound access on TCP port 135 (RPC) through VPC network firewall rules can increase opportunities for malicious activities such as hacking (e.g. backdoor command shell), ransomware attacks, and Denial-of-Service (DoS) attacks.&#x20;

To determine if your Google Cloud VPC firewall rules allow unrestricted access on TCP port 135, perform the following actions:

1. &#x20;First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-mobile-project-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-web-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. --filter network command is used for filtering the network and  sort command is used for sorting the priority.

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                   DISABLED   DIRECTION   SOURCE_RANGES     ALLOW
cc-web-allow-rdp       False      INGRESS     ['0.0.0.0/0']     tcp:3389
cc-web-allow-rpc       False      INGRESS     ['0.0.0.0/0']     tcp:135
```

Check the compute firewall-rules list command output for any active firewall rules  with the DIRECTION set to INGRESS, SOURCE\_RANGES set to \['0.0.0.0/0'], and ALLOW set to tcp:135 or tcp:0-65535. If one or more rules match the search criteria, there are VPC network firewall rules that allow unrestricted access on TCP port 135, therefore the RPC-based access to the associated Google Cloud VM instances is not restricted/secured.

### **11 )  Check for Unrestricted SMTP Access**

Allowing unrestricted inbound/ingress access on TCP port 25 (SMTP) using VPC network firewall rules can increase opportunities for malicious activities such as hacking, spamming, Shellshock and Distributed Denial-of-Service (DDoS) attacks.

To determine if your Google Cloud VPC firewall rules allow unrestricted access on TCP port 25, perform the following operations:

1. &#x20;First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-web-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. _--filter network_ command is used for filtering the network and  _sort_ command is used for sorting the priority

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                   DISABLED   DIRECTION   SOURCE_RANGES     ALLOW
cc-app-allow-smtp      False      INGRESS     ['0.0.0.0/0']     tcp:25 
cc-app-allow-http      False      INGRESS     ['0.0.0.0/0']     tcp:80
cc-app-allow-https     False      INGRESS     ['0.0.0.0/0']     tcp:443
```

Check the compute firewall-rules list command output for any enabled firewall rules  with the DIRECTION set to INGRESS, SOURCE\_RANGES set to \['0.0.0.0/0'], and ALLOW set to tcp:25 or tcp:0-65535. If one or more rules match the search criteria, there are VPC network firewall rules that allow unrestricted access on TCP port 25, therefore the SMTP access granted to the associated Google Cloud resources is not restricted or secured.\


### **12 ) Check for Unrestricted SQL Server Access**

Allowing unrestricted inbound/ingress access on TCP port 1433 (Microsoft SQL Server) via VPC network firewall rules can increase opportunities for malicious activities such as hacking, brute-force attacks, and SQL injection attacks.

To determine if your Google Cloud VPC firewall rules allow unrestricted access on TCP port 1433, perform the following operations:

1. First list all the projects from your google cloud .we will use the following command.

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-web-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. --filter network command is used for filtering the network and  sort command is used for sorting the priority.

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                   DISABLED   DIRECTION   SOURCE_RANGES     ALLOW
cc-app-allow-rdp       False      INGRESS     ['0.0.0.0/0']     tcp:3389
cc-app-allow-mssql     False      INGRESS     ['0.0.0.0/0']     tcp:1433
```

Check the compute firewall-rules list command output for any enabled firewall rules with the DIRECTION set to INGRESS, SOURCE\_RANGES set to \['0.0.0.0/0'], and ALLOW set to tcp:1433 or tcp:0-65535. If one or more rules match the search criteria, there are VPC network firewall rules that allow unrestricted access on TCP port 1433, therefore the SQL Server access granted to the associated Google Cloud resources is not restricted/secured.

### **13 ) Check for Unrestricted SSH Access**

&#x20;Exposing Secure Shell (SSH) port 22 to the Internet can increase opportunities for malicious activities such as hacking, Man-In-The-Middle attacks (MITM) and brute-force attacks, therefore it is strongly recommended to configure your Google Cloud VPC firewall rules to limit inbound traffic on TCP port 22 to known IP addresses only.

To determine if your VPC firewall rules allow unrestricted access on TCP port 22 (SSH), perform the following operations:

1. &#x20;First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-app-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. --filter network command is used for filtering the network and  sort command is used for sorting the priority.

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The command output should return the information available for the existing VPC firewall rules:

```
NAME                   DISABLED  DIRECTION  SOURCE_RANGES     ALLOW
cc-web-allow-icmp      True      INGRESS    ['0.0.0.0/0']     icmp
cc-web-allow-internal  False     INGRESS    ['10.128.0.0/9']  tcp:0-65535,udp:0-65535,icmp
cc-web-allow-ssh       False     INGRESS    ['0.0.0.0/0']     tcp:22
```

Check the compute firewall-rules list command output for any enabled firewall rules  with the DIRECTION set to INGRESS, SOURCE\_RANGES set to \['0.0.0.0/0'], and ALLOW set to tcp:22 or tcp:0-65535. If one or more rules match the search criteria, there are VPC firewall rules that allow unrestricted access on TCP port 22, defined for the selected VPC, therefore the SSH access to the associated Google Cloud VM instances is not secured.

### **14 ) Check for VPC Firewall Rules with Port Ranges**

&#x20;Opening range of ports within your VPC network firewall rules is not a good practice because it can allow attackers to use port scanners and other probing techniques to identify services running on your instances and exploit their vulnerabilities.

To determine if your VPC network firewall rules are using range of ports to allow inbound traffic, perform the following operations:

1. First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-dev-project-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-app-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. --filter network command is used for filtering the network and  sort command is used for sorting the priority.

```
gcloud compute firewall-rules list --filter network=<VPC network name>  --sort-by priority --    format=table"(name,disabled,direction,sourceRanges,allowed[].map().firewall_rule().list())"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                 DISABLED  DIRECTION  SOURCE_RANGES  ALLOW
network-allow-http   False     INGRESS    ['0.0.0.0/0']  tcp:80
network-allow-https  False     INGRESS    ['0.0.0.0/0']  tcp:0-65535
```

Check the compute firewall-rules list command output for any enabled firewall rules  with the DIRECTION set to INGRESS and ALLOW set to a range or ports such as tcp:0-65535 and tcp:80-8080. If one or more rules match the search criteria, there are VPC network firewall rules that are using range of ports to allow inbound traffic, therefore the access to the associated cloud resources is not secured (restricted).

### **15 ) Default VPC Network In Use**

A default VPC might be suitable for getting started quickly with your GCP project, however, when you deploy complex, production applications and use multi-tier architectures, you may need to keep parts of your network private or customize the network model, therefore it is recommended to create a non-default VPC that suits your specific project requirements.

To determine if the default Virtual Private Cloud (VPC) is being used within your GCP projects, perform the following actions:

1. List all the projects from your google cloud .we will use the following command.

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-dev-project-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the selected project:

```
NAME
default
```

If the compute networks list command output lists default as the name of one of the available networks in the project, as shown in the example above, the default Virtual Private Cloud is being used within the selected Google Cloud Platform  project.

### **16 ) Enable Cloud DNS Logging for VPC Networks**

Cloud DNS logging is disabled by default on each Google Cloud VPC network. By enabling monitoring of Cloud DNS logs, you can increase visibility into the DNS names requested by the clients within your VPC network. Cloud DNS logs can be monitored for anomalous domain names and evaluated against threat intelligence.

To determine if Cloud DNS logging is enabled for all your VPC networks, perform the following operations:

1. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-dev-project-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the names of the VPC networks created for the specified project:

```
NAME
cc-prod-vpc-network
cc-main-vpc-network
```

5. Run _**dns policies list**_ command using the name of the VPC network that you want to examine as the filtering parameter, to describe the name of the DNS policy associated with the specified Virtual Private Cloud :

```
gcloud dns policies list --project cc-main-project-123123 --format='value(name)' --filter='networks[].networkUrl ~ cc-prod-vpc-network'
```

6. The command output should return the name of the associated DNS policy:

```
cc-prod-dns-policy
```

7. Run _**dns policies describe**_ command  using the name of the DNS server policy that you want to examine as the identifier parameter, to describe the status of the DNS logging feature for the selected DNS policy:

```
gcloud dns policies describe cc-prod-dns-policy --format="value(enableLogging)"
```

8. The command output should return the status of the Cloud DNS logging feature (**True** for enabled, **False** for disabled):

```
False
```

If the _**dns policies describe**_ command output returns **False**, the Cloud DNS logging is not enabled for the selected VPC network.

### **17 ) Enable Logging for VPC Firewall Rules**

Firewall rule logging allows you to verify, analyze, and audit the effects of your VPC firewall rules on your cloud resources.

To determine if logging is enabled for your VPC network firewall rules, perform the following operations:

1. &#x20;First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-app-stack-network
```

5. Run the _gcloud compute firewall-rules list_ command which displays all Compute Engine firewall rules in a project. --filter network command is used for filtering the network and  sort command is used for sorting the priority.

```
gcloud compute firewall-rules list  --filter network=cc-web-stack-network --sort-by priority  --format=table"(name,disabled,direction,logConfig)"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                    DISABLED  DIRECTION  LOG_CONFIG
cc-allow-egress-access  False     EGRESS     {'enable': False}
cc-allow-mysql-access   False     INGRESS    {'enable': False}
cc-allow-ssh-access     False     INGRESS    {'enable': False}
```

Check the LOG\_CONFIG configuration attribute value for any enabled firewall rules ) returned by the compute firewall-rules list command output. If the LOG\_CONFIG attribute value is set to {'enable': False}, as shown in the example above, the rule logging is not enabled for the selected Google Cloud VPC network firewall rule.

### **18 )  Enable VPC Flow Logs for VPC Subnets**

By default, the VPC Flow Logs feature is disabled when a new VPC network subnet is created. Once enabled, VPC Flow Logs will start collecting network traffic data to and from your Virtual Private Cloud (VPC) subnets, logging data that can be useful for understanding network usage, network traffic expense optimization, network forensics, and real-time security analysis. To enhance Google Cloud VPC network visibility and security it is strongly recommended to enable Flow Logs for every business-critical or production VPC subnet.

To determine if VPC Flow Logs is enabled for all your VPC network subnets, perform the following operations:

1. &#x20;First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-app-stack-network
```

5. Run compute _**networks subnets list**_ command  using the name of the VPC network that you want to examine as identifier parameter and custom filtering to describe all the all the subnets created within the selected Virtual Private Cloud :

```
gcloud compute networks subnets list	--network=cc-web-stack-network	--format=table"(name,region)"
```

6. The command output should return the name and the region of each VPC subnet available inside the selected network:

```
NAME                     REGION
cc-web-stack-network-01  us-central1
cc-web-stack-network-02  europe-west1
cc-web-stack-network-03  us-west1
cc-web-stack-network-04  asia-east1
cc-web-stack-network-05  us-east1
cc-web-stack-network-06  asia-southeast1
cc-web-stack-network-07  us-west2
cc-web-stack-network-08  us-east4
cc-web-stack-network-09  australia-southeast1
```

7. Run _**compute networks subnets describe**_ command using the name and the region of each VPC subnet that you want to examine as identifier parameters, to describe the VPC Flow Logs configuration status for the selected Virtual Private Cloud  subnet:

```
gcloud compute networks subnets describe cc-web-stack-network-01	--region us-central1	--format json | jq '.enableFlowLogs'
```

8. The command output should return the requested configuration status :

```
false
```

If the **compute networks subnets describe** command output returns **null** or **false**, as shown in the example above, the Flow Logs feature is not enabled for the selected VPC subnet.

### **19) Exclude Metadata from Firewall Logging**

VPC firewall logging allows you to verify, analyze, and audit the effects of your firewall rules on your cloud resources. By default, metadata is added within the firewall rule log files. You can significantly reduce the log files size and cut down on storage costs by not including this additional data.

To determine if logging metadata is included within your VPC firewall log files, perform the following operations:

1. &#x20;First list all the projects from your google cloud .we will use the following command

```
gcloud projects list --format="table(projectId)"
```

2. The command output should return the requested GCP project identifiers (IDs):

```
PROJECT_ID
cc-project5-stack-123123
cc-web-platform-stack-112233
```

3. Now we will run a command which list displays all Google Compute Engine networks in a project.

```
gcloud compute networks list  --project <project name>  --format="table(name)"
```

4. The command output should return the name(s) of the VPC network(s) created for the specified project:

```
NAME
cc-web-stack-network
cc-internal-vpc-network
```

5. Run _**compute firewall-rules list**_ command  using the name of the VPC network that you want to examine as identifier parameter and custom filtering to list all the firewall rules  defined for the selected Virtual Private Cloud :

```
gcloud compute firewall-rules list	--filter network=cc-web-stack-network	--sort-by priority	--format=table"(name,disabled,logConfig)"
```

6. The command output should return the requested information available for the existing VPC firewall rules:

```
NAME                   DISABLED  LOG_CONFIG
cc-allow-http-access   False     {'enable': True, 'metadata': 'INCLUDE_ALL_METADATA'}
cc-allow-https-access  False     {'enable': True, 'metadata': 'INCLUDE_ALL_METADATA'}
```

Check the LOG\_CONFIG configuration attribute value for any enabled firewall rules returned by the compute firewall-rules list command output. If the LOG\_CONFIG attribute value is set to {'enable': True, 'metadata': 'INCLUDE\_ALL\_METADATA'}, as shown in the example above, the firewall logging is enabled and the logging metadata is included within the VPC network firewall rule log files.