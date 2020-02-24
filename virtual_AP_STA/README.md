## Introduction

To create a virtual wireless interface in linux, you can use the kernel module **mac80211_hwsim** to create
wireless interfaces, **dnsmasq** to provide DHCP capabilities and **hostapd** to create AP's and authentication servers.

## Configuration

The linux kernel must support the module **mac80211_hwsim**, you can verify it with:

```sh
find /lib/modules/$(uname -r)/ -iname "*$1*.ko*" | cut -d/ -f5- | grep mac80211a
```
 
if something is returned then the module is supported. Now try:

```sh
which hostapd
```

to verify if **hostapd** is available, else you can install it with:

```sh
apt-get install hostapd
```

and if **dnsmasq** is not installed, you can install it with:

```sh
apt-get install dnsmasq
```

Your system must be ready for creating virtual wireless interfaces.

## creating virtual wireless interfaces, AP and connecting to it
run the following to create two virtual interfaces *(wlan0 and wlan1)*:

```sh
modprobe mac80211_hwsim radios=2
```

create the file **hostapd.conf** with this content:

```sh
interface=wlan0
driver=nl80211
ssid=TestAP
hw_mode=g
channel=6
macaddr_acl=0
ignore_broadcast_ssid=0
auth_algs=1
wpa=2
wpa_passphrase=1234567890
wpa_key_mgmt=WPA-PSK WPA-PSK-SHA256
```

and run:

```sh
hostapd hostapd.conf
```

Now if you scan the wireless AP with wlan1:

```sh
iwlist wlan1 scan
```

**(TODO)**

## Reference

Use this [link](https://thehackingfactory.com/crear-un-punto-de-acceso-con-hostapd-y-dnsmasq) as reference for the process
