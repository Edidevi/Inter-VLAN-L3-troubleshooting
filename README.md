# Inter-VLAN-troubleshooting
<img width="522" height="366" alt="image" src="https://github.com/user-attachments/assets/01d6726b-60a9-4a23-941f-e508577faf98" />

Troubleshooting scenarios for an enterprise inter VLAN scenario


## 🎫 Incident Ticket #INC-00851
**Priority:** `P2 — High` **Reported by:** `James T. IT` **Assigned to:** `Network Support` **Time logged:** `09:15 AM`
<img width="300" height="300" align="left" alt="image" src="https://github.com/user-attachments/assets/94da311f-1d76-4e82-911b-1a8fe6ba70ef" />


## 🎫 Incident Ticket #INC-00917
**Priority:** `P2 — High` **Reported by:** `Derek N., Sales` **Assigned to:** `Network Support` **Time logged:** `15:38 PM`
<img width="300" height="300" align="left" alt="image" src="https://github.com/user-attachments/assets/d7958a35-3bb1-4ded-aa15-4c1c4173f31f" />
<img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/01d6726b-60a9-4a23-941f-e508577faf98" />

## 🎫 Incident Ticket #INC-00902
**Priority:** `P2 — High` **Reported by:** `Karen W., Finance` **Assigned to:** `Network Support` **Time logged:** `11:04 AM`
<img width="300" height="300" align="left" alt="image" src="https://github.com/user-attachments/assets/6a8e1fd8-fe62-49e5-9f4e-656e04b2e36e" />
<img width="300" height="300" align="left" alt="image" src="https://github.com/user-attachments/assets/91072571-a053-4278-8a73-0d231f9b5477" />

**<ins>Ticket Key Points:<ins>**
    
- Finance gets IP addresses, but IPs are strange, don't belong to expected subnet</li>
- Finance pings to OPs work sometimes, but inconsistent</li>
- Operations see occasional weird behaviour when trying to reach finance</li>
- Network team did some trunk optimisation work between D1 and access switches yesterday
 
<br>

**<ins>Step 1: Layer 1 <ins>**
    
- Checked cabling on every connection, to make sure right cables were used 
- Noticed **incorrect cabling** between **SW-Finance**, **SW-OPs** and the **D1**.
- Replaced **crossover cable** with a straight through cable. Having incorrect cabling could cause inconssistent pings due to collisions from to pins transmitting at same time. Maybe the network team made a mistake yesterday?
- The next step was the interfaces, to check for possible **down interfaces**. I useed `show ip interface brief` on the two swtiches, as well as the layer 3 switch.
- I checked the **SW-Finance** first, on which there were no alarming things. Connected interfaces were up/up as expected 
- I then checked **SW-OPs** and i got a native VLAN mismatch message for the trunk between `GigabitEthernet0/1` on **SW-OP** and `FastEthernet0/1` on **D1** 
<img width="797" height="120" alt="image" src="https://github.com/user-attachments/assets/47db9b83-8186-4407-b1ba-6286cfdc5590" />

- I logged into interface `Fast Ethernet 0/1` from global configuration mode and typed `switchport trunk native Vlan 30` to chnage **D1s** interface vlan to **SW-OPs** interface Vlan, Vlan 30

Noticed **incorrect cabling** between **SW-Finance**, **SW-OPs** and the **D1**.</li>
      Replaced **crossover cable** with a straight through cable. Having incorrect cabling could cause inconssistent pings due to collisions from to pins transmitting at same time. Maybe the network team made a mistake yesterday?</li>
      <li>The next step was the interfaces, to check for possible **down interfaces**. I useed `show ip interface brief` on the two swtiches, as well as the layer 3 switch.
      <li>I checked the **SW-Finance** first, on which there were no alarming things. Connected interfaces were up/up as expected </li>
      <li>I then checked **SW-OPs** and i got a native VLAN mismatch message for the trunk between `GigabitEthernet0/1` on **SW-OP** and `FastEthernet0/1` on **D1** <br>
      </ul>
      <img width="797" height="120" alt="image" src="https://github.com/user-attachments/assets/47db9b83-8186-4407-b1ba-6286cfdc5590" />
</ol>


</ol>


<img width="797" height="120" alt="image" src="https://github.com/user-attachments/assets/47db9b83-8186-4407-b1ba-6286cfdc5590" />
<img width="688" height="363" alt="image" src="https://github.com/user-attachments/assets/ae15cfe5-44db-4487-b251-5b51f0addf49" />
<img width="598" height="457" alt="image" src="https://github.com/user-attachments/assets/671a3580-beed-48a2-bef4-eac87ecf0f74" />

![Uploading image.png…]()


