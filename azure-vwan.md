[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# vWAN

## Architectural Point

- In Azure Virtual WAN: ``Spokes do NOT point directly to the firewall.``
- Instead: ``Spokes point to the vHub.``

And the vHub uses:

- Route Tables
- Routing Intent
- Security Provider associations

to decide how traffic flows.

### Route Tables vs Routing Intent  
| Concept           | Route Tables                    | Routing Intent                                                      |
| ------------------- | ------------------------------- | ------------------------------------------------------------------- |
| Conceptual Definition | You explicitly define where traffic should be routed    | You define the traffic policy/intention               |
| Purpose             | Define where traffic is routed  | Define which traffic must pass through a firewall/security provider |
| Type                | Routing configuration           | Security/routing policy abstraction                                 |
| Level               | Low-level routing               | High-level centralized policy                                       |
| Configuration style | Manual                          | Automated / Intent-based                                            |
| Typical use         | Route traffic to a next hop     | Force Internet/Private traffic inspection                           |
| Works with          | Routes and next hops            | Security Providers (Azure Firewall, FortiGate, etc.)                |
| Scope               | Individual routes               | Traffic categories                                                  |
| Example             | `0.0.0.0/0 → Azure Firewall IP` | `Internet Traffic → Azure Firewall`                                 |
| Complexity          | More operational work           | Simpler centralized management                                      |
| Best fit            | Custom routing scenarios        | Enterprise vWAN security architectures                              |
| How it works                       | Manual routing configuration                            | Automated policy-based routing                        |
| Example Definition                 | “Send this network to this next hop”                    | “All Internet traffic must pass through the firewall” |
| Example                            | `10.50.0.0/16 → VPN Gateway`<br/>`0.0.0.0/0 → Firewall` | `Internet Traffic → Azure Firewall`                   |
| Management Style                   | Manual                                                  | Centralized and automated                             |
| Operational Complexity             | Higher                                                  | Lower                                                 |
| Typical Usage                      | Custom routing scenarios                                | Enterprise security enforcement in vWAN               |
| Azure automatically creates routes | No                                                      | Yes                                                   |


### Routing Intent types

| Routing Intent Type | Description                                       | Typical Example                                                                 |
| ------------------- | ------------------------------------------------- | ------------------------------------------------------------------------------- |
| Internet Traffic    | Controls all Internet-bound traffic (`0.0.0.0/0`) | Force all outbound Internet traffic through Azure Firewall or FortiGate         |
| Private Traffic     | Controls internal/private traffic                 | Force Spoke-to-Spoke, VPN, ExpressRoute, and On-Prem traffic through a firewall |



### Simplified Traffic Flow

````
Spoke
   ↓
vHub Route Table
   ↓
Routing Intent
   ↓
Security Provider
   ↓
Firewall
   ↓
Internet / OnPrem / Other Spokes
````



## Routing Intent vs Manual UDRs

| Feature              | Routing Intent                                                                   | Manual UDRs                                                      |
| -------------------- | -------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| Definition           | Centralized vWAN policy that forces traffic through a firewall/security provider | Manual route configuration defining where traffic should be sent |
| Scope                | Azure Virtual WAN Hub                                                            | Individual subnet                                                |
| Management           | Centralized                                                                      | Distributed                                                      |
| Scalability          | High                                                                             | Medium / Low                                                     |
| Maintenance          | Simple                                                                           | Complex at scale                                                 |
| Firewall abstraction | Yes                                                                              | No                                                               |
| Best for             | Enterprise Landing Zones                                                         | Small/custom environments                                        |
| Dynamic routing      | Native integration                                                               | Manual updates required                                          |
| Future FW migration  | Easy                                                                             | Operationally complex                                            |


**Examples**
| Scenario                      | Routing Intent                      | Manual User Defined Route "UDR"                  |
| ----------------------------- | ----------------------------------- | ---------------------------- |
| Internet traffic inspection   | `Internet Traffic → Azure Firewall` | `0.0.0.0/0 → Firewall IP`    |
| Future migration to FortiGate | Change Security Provider in vHub    | Modify UDRs in all spokes    |
| Spoke-to-Spoke inspection     | `Private Traffic → FortiGate`       | Custom routes per subnet     |
| VPN/OnPrem inspection         | Centralized through vHub            | Manual routing rules         |
| 200 spokes                    | Managed centrally                   | Hundreds of UDR associations |


**Routing Intent**
````
Spokes
   ↓
vWAN Hub
   ↓
Routing Intent
   ↓
Security Provider
   ↓
Firewall
````

**Manual UDRs**
````
Spoke Subnet
   ↓
UDR
0.0.0.0/0 → Firewall IP
   ↓
Firewall
````

Routing Intent is a feature in Azure Virtual WAN that allows you to define:

``"What type of traffic must always pass through a security device"``

without manually configuring hundreds of routes or UDRs.

It is basically a centralized traffic steering policy inside the vHub.



### Conceptually

Without Routing Intent:

````
Spokes
   ↓
Manual UDRs
   ↓
Firewall
````

With Routing Intent:

````
Spokes
   ↓
vHub
   ↓
Routing Intent
   ↓
Firewall / NVA
````

The spokes do not know which firewall is being used.

The vHub decides where traffic should go.

Main Purpose

Routing Intent is used to force traffic through:

Azure Firewall
FortiGate NVA
Palo Alto
CheckPoint
Other supported Security Providers
Main Traffic Types

Routing Intent mainly works with two traffic categories.

1. Internet Traffic

This means:

0.0.0.0/0

All Internet-bound traffic.

Example:

Internet Traffic
   ↓
Azure Firewall

This forces all outbound Internet traffic from spokes through the firewall.

2. Private Traffic

This includes:

RFC1918 traffic
Spoke-to-Spoke
On-Prem
VPN
ExpressRoute

Example:

Private Traffic
   ↓
FortiGate NVA

This forces internal/private traffic through the security provider.

What Problem Does Routing Intent Solve?

Without Routing Intent, you would need:

Hundreds or thousands of UDRs

distributed across:

spokes
subnets
environments

That becomes difficult to maintain.

Routing Intent centralizes everything inside the vHub.
