## 🔧 **Basic Configuration Summary for Routers R1, R2, and R3**

### 🔹 Part 1: Basic Device Configuration & OSPF Setup

#### 1. **Configure Hostnames and IP Addresses**

```bash
enable
configure terminal
hostname R1

interface g0/0/0
ip address 10.1.1.1 255.255.255.0
no shutdown

interface g0/0/1
ip address 192.168.1.1 255.255.255.0
no shutdown

no ip domain-lookup
```

#### 2. **Configure OSPF Routing**

```bash
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
network 10.1.1.0 0.0.0.3 area 0
passive-interface g0/0/1
```

*(Repeat OSPF with appropriate interfaces and networks on R2 and R3)*

#### 3. **Verify OSPF**

```bash
show ip ospf neighbor
show ip route
```

#### 4. **Configure IP Settings on PCs**

* Assign static IPs to PCs in the same subnet as the router's LAN interface.

  * Example for PC-A:

    * IP: `192.168.1.10`
    * Subnet: `255.255.255.0`
    * Gateway: `192.168.1.1`

#### 5. **Test Connectivity**

```bash
ping [Other PC or router IP]
```

#### 6. **Save Configuration**

```bash
copy running-config startup-config
```

---

## 🔐 **Part 2: Configure and Encrypt Passwords**

### 1. **Set Encrypted Enable Password**

```bash
enable algorithm-type scrypt secret cisco12345
security passwords min-length 10
```

### 2. **Console Line Configuration**

```bash
line console 0
password ciscocon
exec-timeout 5 0
login
logging synchronous
```

### 3. **AUX Port Configuration**

```bash
line aux 0
password ciscoauxpass
exec-timeout 5 0
login
```

### 4. **VTY (Telnet) Line Configuration**

```bash
line vty 0 4
password ciscovtypass
exec-timeout 5 0
transport input telnet
login
```

### 5. **Encrypt All Cleartext Passwords**

```bash
service password-encryption
```

### 6. **Set Login Warning Banner**

```bash
banner motd $Unauthorized access strictly prohibited!$
```

---

## 👤 **Part 3: Configure Enhanced Username/Password Security**

### 1. **Create a New User with Encrypted Password**

```bash
username user01 algorithm-type scrypt secret user01pass
```

### 2. **Configure Console to Use Local Login**

```bash
line console 0
login local
```

---

## 🔐 **Part 4: Configure SSH Access**

### 1. **Set Domain Name**

```bash
ip domain-name mynetwork.local
```

### 2. **Generate RSA Keys for SSH**

```bash
crypto key generate rsa
# Choose key size: 1024 or 2048 bits
```

### 3. **Enable SSH on VTY Lines**

```bash
line vty 0 4
transport input ssh
login local
```

### 4. **Verify SSH Configuration**

```bash
show ip ssh
```

---

## ✅ **Final Steps**

* Save configuration:

```bash
copy running-config startup-config
```

* Test SSH:

```bash
ssh -l user01 [Router IP]
```
