# Wifi Router Openwrt

>   ### ___Needs___
>   - RaspberryPi 4
>   - Power Supply
>   - MicroSd at least 32 GB
>   - MicroSd Card Reader
>   - Ethernet Cable (cat5)
>   - Usb Wifi Adapter

>   ### ___Configuration___
>   1. Download OPENWRT <a href="https://firmware-selector.openwrt.org/?version=23.05.0&target=bcm27xx%2Fbcm2711&id=rpi-4">SysUpgrade Version (EXT4)</a>
>   2. Install raspbery pi imager Or Using <a href="https://etcher.balena.io/" target="_blank">Balena</a>
>   3. change IP to 192.168.1.10
>   4. connect via `ssh root@192.168.1.1`
>   5. change password with 
>   6. ` cd /etc/config && ls`
>   7. ` cp wireless wireless.bk`
>   8. ` cp network network.bk`
>   9. ` cp firewall firewall.bk`

>
## ___Configs___
###  ___Network___ 

```bash
$ vi network
```

```bash

config interface 'loopback'
        option device 'lo'
        option proto 'static'
        option ipaddr '127.0.0.1'
        option netmask '255.0.0.0'

config globals 'globals'
        option ula_prefix 'fd3c:3698:d05c::/48'

config device
        option name 'br-lan'
        option type 'bridge'
        list ports 'eth0'

config interface 'lan'
        option device 'br-lan'
        option proto 'static'
        option ipaddr '192.168.1.2'
        option netmask '255.255.255.0'
        option ip6assign '60'
        option force_link '1'

config interface 'wwan'
        option proto 'dhcp'
        option preerdns '0'
        option dns '1.1.1.1 8.8.8.8'

config interface 'vpnclient'
        option ifname 'tun0'
        option proto 'none'
```

### ___Firewall___

```bash 
   vi firewall
   config zone
          option input ACCEPT
```


```bash 
   $ reboot
```


### ___Connecting Again___
***

```bash
 $ ssh root@192.168.1.2
 $ cd /etc/config
  
 $ vi /etc/config/wireless
```
```bash 

  config wifi-device 'radio0'
        option type 'mac80211'
        option path 'platform/soc/fe300000.mmcnr/mmc_host/mmc1/mmc1:0001/mmc1:0001:1'
        option channel '7'
        option htmode 'HT20'
        option disabled '0'
        option hwmode '11g'
        option short_gi_40 '0'

  config wifi-iface 'default_radio0'
        option device 'radio0'
        option network 'lan'
        option mode 'ap'
        option ssid 'OpenWrt'
        option encryption 'none'
```

```bash
 $ uci commit wireless
 $ wifi
```

#### Internet to Raspberry

> in browser call <a href="http://192.168.1.2">192.168.1.2</a>
> 
> - go to wifi radio0
> - scan wifi
> - select wifi
> - ∆ tick Replace wireless configuration
> - save and apply
> ### go to the terminal 
> #### install tools and drivers
> 
> 
```bash
  reboot
  ssh root@192.168.1.2
    
  opkg update
```
```bash

  opkg install kmod-usb-core kmod-usb-uhci kmod-usb-ohci kmod-usb2 usbutils openvpn-openssl luci-app-openvpn
```
```bash
  opkg install kernel kmod-mac80211 kmod-usb-core mt7601u-firmware opkg install mt7601u-firmware mt7601u kmod-mt7601u
```
```bash
  opkg install nano curl
```
