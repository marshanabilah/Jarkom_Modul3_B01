# Jarkom_Modul3_B01

3. Client pada subnet 1 mendapatkan range 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 
dan 6. peminjaman alamat IP selama 5 menit
```nano /etc/dhcp/dhcpd.conf```

```
subnet 192.168.0.0 netmask 255.255.255.0 {
   range 192.168.0.10 192.168.0.100;
   range 192.168.0.110 192.168.0.200;
   option routers 192.168.0.1;
   option broadcast-address 192.168.0.255;
   option domain-name-servers 202.46.129.2, 10.151.83.18;
   default-lease-time 300;
   max-lease-time 7200;
}
```

4. Client pada subnet 2 mendapatkan range IP 192.168.0.50 sampai 192.168.1.70 dan 6. peminjamaman alamat 
IP selama 10 menit
```nano /etc/dhcp/dhcpd.conf```

```
subnet 192.168.1.0 netmask 255.255.255.0 {
   	range 192.168.1.50 192.168.1.70;
    	option routers 192.168.1.1;
    	option broadcast-address 192.168.1.255;
    	option domain-name-servers 202.46.129.2, 10.151.83.18;
    	default-lease-time 600;
    	max-lease-time 7200;
}
```

7. User autentikasi

di Mojokerto

```apt-get install squid```

```mv /etc/squid/squid.conf /etc/squid/squid.conf.bak```

```apt-get install apache2-utils```

```httpasswd -c /etc/squid/passwd userta_b01``` kemudian isikan password ```inipassw0rdta_b01```

```nano /etc/squid/squid.conf```

```
acl all src 0.0.0.0/0.0.0.0

include /etc/squid/acl.conf
http_port 8080
visible_hostname mojokerto

auth_param basic program /usr/lib/squid/ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
```

8. penggunaan internet Anri dibatasi menjadi Selasa - Rabu pukul 13.00-18.00 dan 9. Selasa - Kamis pukul 21.00-09.00

```nano /etc/squid/acl.conf```

```
acl AVAILABLE_WORKING time TW 13:00-18:00
acl AVAILABLE_WORKING2 time TWH 21:00-23:59
acl AVAILABLE_WORKING3 time WHF 00:00-09:00
```

```nano /etc/squid/squid.conf```

```acl all src 0.0.0.0/0.0.0.0

include /etc/squid/acl.conf
http_port 8080
visible_hostname mojokerto

auth_param basic program /usr/lib/squid/ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED

http_access allow USERS AVAILABLE_WORKING
http_access allow USERS AVAILABLE_WORKING2
http_access allow USERS AVAILABLE_WORKING3
```

9. pada saat Anri mengakses google.com, maka akan redirect ke monta.if.its.ac.id

```nano /etc/squid/squid.conf```

```
acl all src 0.0.0.0/0.0.0.0

include /etc/squid/acl.conf
http_port 8080
visible_hostname mojokerto

auth_param basic program /usr/lib/squid/ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED

acl REDIRECT dstdomain .google.com
deny_info http://monta.if.its.ac.id/ REDIRECT
http_access deny REDIRECT

http_access allow USERS AVAILABLE_WORKING
http_access allow USERS AVAILABLE_WORKING2
http_access allow USERS AVAILABLE_WORKING3
```
