---
layout: troubleshooting
title: Inter-VLAN L3 Troubleshooting
description: "Enterprise inter-VLAN support scenarios. Real-world incident tickets simulating network faults across a Layer 3 switched environment."
diagram: https://github.com/user-attachments/assets/01d6726b-60a9-4a23-941f-e508577faf98
 
concepts:
  - Inter-VLAN Routing
  - Native VLAN Mismatch
  - DHCP Troubleshooting
  - Layer 1 to Layer 3 Methodology
  - SVI Verification
 
environment:
  - "1x Cisco 3560 Layer 3 Switch (D1)"
  - "2x Cisco Catalyst 2960 (SW-Finance, SW-OPs)"
  - "End Devices — Finance & Operations PCs"
  - "Cisco Packet Tracer"
 
incidents:
  - id: "Incident Ticket #INC-00902"
    priority: "P2 — High"
    reported_by: "Karen W., Finance"
    assigned_to: "Network Support"
    time: "11:04 AM"
 
    symptoms:
      - "Finance gets IP addresses, but IPs are strange and don't belong to the expected subnet"
      - "Finance pings to OPs work sometimes, but are inconsistent"
      - "Operations see occasional weird behaviour when trying to reach Finance"
      - "Network team did trunk optimisation work between D1 and access switches yesterday"
 
    steps:
      - layer: "Layer 1"
        title: "Physical Layer & Cabling Check"
        findings:
          - "Started troubleshooting systematically at Layer 1 by checking cabling on every connection."
          - "Noticed <strong>incorrect cabling</strong> between <strong>SW-Finance</strong>, <strong>SW-OPs</strong> and <strong>D1</strong>. A crossover cable was being used where a straight-through cable was required."
          - "Replaced the crossover cable with a straight-through cable. Incorrect cabling could cause inconsistent pings due to collisions from two pins transmitting simultaneously — possibly a mistake made during yesterday's trunk optimisation work."
 
      - layer: "Layer 2"
        title: "VLAN & Trunk Verification"
        findings:
          - "Checked interfaces using <code>show ip interface brief</code> on both access switches and D1."
          - "<strong>SW-Finance</strong> — all connected interfaces were up/up as expected. No issues found."
          - "<strong>SW-OPs</strong> — received a native VLAN mismatch message for the trunk between <code>GigabitEthernet0/1</code> on SW-OPs and <code>FastEthernet0/1</code> on D1."
          - "Logged into interface <code>FastEthernet 0/1</code> on D1 and ran <code>switchport trunk native vlan 30</code> to align D1's native VLAN with SW-OPs' native VLAN (VLAN 30)."
        images:
          - src: https://github.com/user-attachments/assets/47db9b83-8186-4407-b1ba-6286cfdc5590
            caption: "Native VLAN mismatch error on GigabitEthernet0/1 between SW-OPs and D1"
 
      - layer: "Layer 3"
        title: "SVI & DHCP Verification"
        findings:
          - "Returned to SW-OPs to verify interfaces — all required interfaces confirmed up/up."
          - "Checked D1 SVIs using <code>show ip interface brief</code> — both SVIs were up/up with the correct IP addresses."
          - "Investigated Finance PCs' IP addresses since they were reporting unexpected addresses. PCs had signature Class C private IPs (192.168.x.x) that didn't match the expected subnet."
          - "The previous native VLAN mismatch was likely the cause — DHCP offers from D1 could not reach Finance PCs correctly while the mismatch was active."
          - "Reset DHCP on affected devices by briefly switching to static, then back to DHCP to force a fresh lease. Finance PCs then received correct IP addresses."
        images:
          - src: https://github.com/user-attachments/assets/0c2b5618-2467-4df3-9578-c4ea08dc8f45
            caption: "Finance PC receiving correct DHCP address after fix"
          - src: https://github.com/user-attachments/assets/b112de77-9605-4ea2-a0ed-fbcd8491b488
            caption: "Ops PC address verification"
 
      - layer: "Verify"
        title: "Inter-VLAN Connectivity Test"
        findings:
          - "Performed ping test from Finance VLAN to Operations VLAN to confirm inter-VLAN routing was fully restored."
          - "First ping failed — expected due to ARP resolution on first contact. Subsequent pings were all successful."
        images:
          - src: https://github.com/user-attachments/assets/671a3580-beed-48a2-bef4-eac87ecf0f74
            caption: "Successful inter-VLAN ping results — Finance to Operations"
 
    resolution:
      - "Finance PCs now receive correct IP addresses — DHCP pooling works correctly after native VLAN mismatch was resolved."
      - "Finance to Operations pings are now fully functional and consistent."
 
learnings:
  - "Always start troubleshooting at Layer 1 — physical faults can cause symptoms that appear to be Layer 3 issues."
  - "Native VLAN mismatches on trunk links can silently break DHCP and cause inconsistent connectivity."
  - "DHCP leases may need to be manually refreshed after fixing a VLAN mismatch."
  - "The first ping in a new ARP session will fail — this is expected behaviour, not a fault."
---
