# DigiJED-2 ICT Security Projects

This repository contains projects created during the DigiJED-2 ICT Security course. Projects involve both practical and laboratory work. The practical work includes the following topics: setting up a local area network, a virtual local area network, a VPN, a wireless local area network, and protecting a router connection. The laboratory study included topics such as creating a virtual local area network, securing local area networks, securing wide area networks, and studying single-path routing methods for various metrics.

## Overview of the Course
In the DigiJED-2 ICT Security course, I've gained essential theoretical and practical skills for securing ICT networks. The curriculum covered foundational ICT technology, emphasizing the critical role of security in today's technological landscape. Topics included network vulnerabilities, LAN and WAN security, Access Control Lists, Remote Access Security, DHCP, DNS, ARP, VPNs, VLAN configuration, and routing security. Practical exercises, including network scanning concepts, enhanced my ability to identify and mitigate potential threats. Overall, the course has equipped me with a robust skill set to contribute effectively to ICT network security.

## More about the projects

### [Practice](Practice)

- Practice LAN:
  
  During this practical lesson, two local area networks were configured using different devices, and address assignment was configured both statically and dynamically using the DHCP protocol.

- Practice VLAN:
  
  During this practical lesson, three virtual LANs were configured, with and without DTP, and the VTP protocol was also used. It was possible to connect from each virtual network to another, as well as to connect to the global network with each virtual network

- Practice Secure Router:
  
  During this practical lesson, security was ensured when connecting to the router directly, or via telnet or ssh protocols, as well as securing the router's privileged mode and encrypting all passwords stored in the router system. 

- Practice VPN:
  
  During this practical lesson, we set up a VPN connection from one of the virtual local area networks to another network. The ISAKMP and IPsec protocols were used in this lab.

- Practice Wi-Fi:
  
  During this practical lesson, we set up a wireless network, use different types of 2.4GHz and 5GHz networks, configure DHCP service on wireless and regular routers, set up a guest network on a wireless router, secure this network, and create an access сontrol list.

### [Labs](Labs)

- [Building virtual local area networks](Labs/Lab_1):
  
In this lab session, we developed proficiency in establishing and securing virtual local area networks (VLANs). Our focus included the creation of access ports and trunk ports, along with configuring routers to facilitate routing between these virtual networks. We delved into the intricacies of Dynamic Trunking Protocol (DTP) and VLAN Trunking Protocol (VTP), recognizing the advantages of automation for simplifying tasks, while acknowledging the potential risks associated with attackers exploiting the vulnerabilities inherent in these protocols.

- [Protection of local networks](Labs/Lab_2):

During the laboratory work, we delved into the implementation of security tools for local networks, focusing on configuring Port Security on Switch 0 in the initial task. This involved setting limits on the maximum number of devices permitted on specific ports, providing a robust means of controlling network access. Two potential security breach scenarios were simulated to assess the effectiveness of the applied measures.

The second task centered around exploring DHCP service protection. Legitimate and illegitimate DHCP servers were configured, and DHCP Snooping was employed to thwart unauthorized DHCP servers from infiltrating the network. Trust ports were established, and speed restrictions were imposed on untrusted switch ports, enhancing overall network security.

The overarching objective of the lab was to acquaint students with fundamental methods for safeguarding local networks and to cultivate practical skills in their application. These acquired skills empower individuals to adeptly manage network security, proactively mitigating potential threats.

- [Protection of global networks](Labs/Lab_3):
- 
Throughout the laboratory session, various elements of the network infrastructure were set up and tested, encompassing remote secure access through SSH, configuration of the RIP routing protocol, and the implementation of the HSRP fault-tolerant routing protocol.

In the initial phase of the lab, it was identified that utilizing the SSH protocol offers a dependable and secure method for remote access to network devices. The incorporation of data encryption and robust authentication mechanisms serves to mitigate the risks associated with unauthorized system access.

Following the configuration in the second stage, the RIP protocol was effectively deployed, leading to an enhancement in the speed of information exchange between routers. The implementation of RIP contributes to the optimization of routing, ensuring the efficient operation of the network.

It was observed that when transitioning from the active to the standby router, a temporary packet loss may occur. However, the HSRP protocol demonstrates high efficiency in fault detection and reliable switching between routers.

To assess the security level, Router Spy was introduced, and access control lists (ACLs) were configured. This approach enables effective control over access to the HSRP protocol, providing protection against potential attacks. The analysis results underscore the significance of appropriately configuring ACLs to safeguard the HSRP protocol and mitigate potential threats.

The overarching objective of the lab was to explore and implement crucial aspects of networking technologies with a focus on security and fault tolerance. The skills acquired during these tasks serve as a valuable asset for proficiently managing local network security, offering the capability to proactively counter potential threats to the infrastructure.

- [Study of single-path routing solutions for different types of metrics](Labs/Lab_4):
This laboratory investigation delved into a comprehensive examination of single-path routing solutions using the GEKKO Optimization Suite in Python. The study considered three distinct variants of routing metrics vectors: RIP, OSPF, and security metrics. The obtained results offer a basis for comparing various facets of routing, enabling the selection of the optimal route based on user-defined requirements.

For the packet transmission from node 1 (sender) to node 6 (receiver), the optimal route was computed for each of the specified metrics. Additionally, three supplementary metrics were established for each route:

    Number of retransmissions (hops).
    Throughput capacity.
    Probability of compromise.

All routes shared the same number of hops, specifically three. The route calculated using RIP metrics aimed for the minimum number of hops, while the distances for the other two routes were matched to that of the RIP-calculated route.

The throughput for routes computed using RIP and security metrics was identical, given that their routes were the same. The route determined by OSPF metrics exhibited the highest minimum throughput among all calculated routes.

Routes calculated using RIP and security metrics shared the same compromise probability, as they followed the same path. Conversely, the route computed using OSPF metrics had the highest probability of compromise. The security metrics route was specifically designed to minimize compromise probability, and the compromise probability values for the RIP metrics route matched those of the security metrics route, as they traversed the same path.

In summary:

    If minimizing distance is the primary objective, RIP is the optimal choice.
    For the highest minimum throughput, OSPF emerges as the best option.
    When security is the utmost priority, the security metric proves to be the most suitable choice.


## License

This repository is provided for educational and reference purposes under the MIT License.
