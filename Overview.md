TBefore Jens “atom” Steube’s (Hashcat’s lead developer) research, when a hacker wanted to crack a WiFi password, they needed to capture a live four-way handshake between a client and a router occurring only during the establishment of the connection. Simply put, the attacker would need to be monitoring the network at the time the user or device connects to the WiFi network. Therefore, a hacker needed to be in a physical location between the access point (router) and the client, hoping that the user would enter the right password and that all four packets of the handshake were sniffed correctly. If a hacker did not want to wait until a victim establishes a connection (which can take hours, who connects to their home network while they are at work?), the attacker could de-authenticate an already-connected user to force the victim to have a new four-way handshake.

Another attack vector is to set up a malicious twin network with the same SSID (network name), hoping that the victim would try to log in to the fake network. A major shortcoming of this is, of course, that it is very noisy (meaning it can be easily traced) and can be easily noticed.

In simple English, if an adversary wanted to hack/crack a WiFi password, they need to be in the right place (between users and a router) at the right time (when users log in) and be lucky (users entered the correct password and all four packets were sniffed correctly).

All of this changed with atom’s groundbreaking research, which exposed a new vulnerability targeting RSN IE (Robust Security Network Information Element) to retrieve a PMKID hash (will be explained in a bit) that can be used to crack the target network password. PMKID is a hash that is used for roaming capabilities between APs. The legitimate use of PMKID is, however, of little relevance for the scope of this blog. Frankly, it makes little sense to enable it on routers for personal/private use (WPA2-personal), as usually there is no need for roaming in a personal network.


Atom’s technique is clientless, making the need to capture a user’s login in real time and the need for users to connect to the network at all obsolete. Furthermore, it only requires the attacker to capture a single frame and eliminate wrong passwords and malformed frames that are disturbing the cracking process.

Plainly put, we do not need to wait for people connecting to their routers for this attack to be successful. We are just in the vicinity of the router/network getting a PMKID hash and trying to crack it.

## How a PMKID is generated?

![[Overview-20241208184416068.webp]]
<center>Figure 1 - Flow of Calculating PMKID hash and PMK</center>

The hash calculation might seem daunting at first glance but let us dive into it.

### Generating a PMK

- We need to generate a <font color="#4f81bd">PMK</font> driven from <font color="#92d050">SSID</font> (the network name) and the <font color="#de7802">Passphrase</font>;.
- Then we generate a <font color="#7030a0">PMKID</font> driven from the <font color="#4f81bd">PMK</font> we generated, the <font color="#00b050">AP MAC</font> address, and the <font color="#00b0f0">client MAC</font> address. 

So let us see where we can find those:

![[Overview-20241208185217505.webp]]
<center>Figure 2 - PMK calculation</center>

- <font color="#de7802">Passphrase</font>– The WiFi password — hence, the part that we are really looking for.
- <font color="#92d050">SSID</font> – The name of the network. It is freely available at the router beacons (Figure 3).
- 4096 – Number of PBKDF2 iterations.

![[Overview-20241208185542147.webp]]
<center>Figure 3 - SSID from a beacon</center>

After a PMK was generated, we can generate a PMKID.

### Computing a PMKID

![[Overview-20241208190008434.webp]]
<center>Figure 4 - PMKID calculation</center>

- <font color="#4f81bd">PMK</font> – What we are searching for, generated above. In WPA2 personal, the PMK is the PSK (will be explained in the next paragraph).
- “PMK Name” – Static string for all PKMIDs.
- <font color="#00b050">MAC_AP</font> – Access Point’s MAC address – This address can be found in any frame send by the router (Figure 3).
- <font color="#00b0f0">MAC_STA</font> – The client’s Mac address can be found in any frame sent by the client’s computer(Figure 3). It can moreover be found in the output of ifconfig\ip a commands.

![[Overview-20241208191023161.webp]]
<center>Figure 5 – PMKID, AP’s MAC, Client’s MAC</center>

- Cracking the <font color="#7030a0">PMKID</font> hash is ultimately just generating/calculating PMKs with the <font color="#9bbb59">SSID</font> and different passphrases.
- Then calculating <font color="#7030a0">PMKID</font> from the <font color="#0070c0">PMK</font> and the other information we obtained. 
- Once we generated a <font color="#7030a0">PMKID</font> equal to the <font color="#7030a0">PMKID</font> that was retrieved from the AP (Figure 2), the hash is cracked.
- The <font color="#de7802">passphrases</font> that were used to generate the right <font color="#0070c0">PMK</font> that the <font color="#7030a0">PMKID</font> was generated from is the correct WiFi password.

**Next step:** [[Hardware requirements]]
