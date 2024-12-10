1. Kali Linux as OS.
2. [hcxtools](https://github.com/kairosart/hcxtools):  To convert  the sniffing results in the form of _pcapng_ into a hashfile format to fit it to _hashcat_.
3. [hashcat](https://hashcat.net/hashcat/):  Password recovery tool.

## Hashcat

Since version 6.0.0, hashcat has been offering the new hash mode 22000. 
  
22000 | WPA-PBKDF2-PMKID+EAPOL
22001 | WPA-PMK-PMKID+EAPOL`

what are the benefits of hash mode 22000?  

- The hash mode 22000 hash line combines PMKIDs and EAPOL MESSAGE PAIRs in a single file  

- Having all the different handshake types in a single file allows for efficient reuse of PBKDF2 to save GPU cycles  

- It is no longer a binary format that allows various standard tools to be used to filter or process the hashes  

- It is no longer a binary format which makes it easier to copy / paste anywhere as it is just text  

- The best tools for capturing and filtering WPA handshake output in hash mode 22000 format (see tools below)

---

In order to be able to use the hash mode 22000 to the full extent, you need the following tools:  

- hcxdumptool v6.0.0 or higher: [https://github.com/ZerBea/hcxdumptool](https://github.com/ZerBea/hcxdumptool)  

- hcxpcapngtool from hcxtools v6.0.0 or higher: [https://github.com/ZerBea/hcxtools](https://github.com/ZerBea/hcxtools)  

- hashcat v6.0.0 or higher: [https://github.com/hashcat/hashcat](https://github.com/hashcat/hashcat)


---
The new hash format 22000 in detail:  
`PROTOCOL*TYPE*PMKID/MIC*MACAP*MACCLIENT*ESSID*ANONCE*EAPOL*MESSAGEPAIR`