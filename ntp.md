# ntp
    restrict 192.168.122.0 mask 255.255.255.0 nomodify

    server 127.127.1.0
    fudge  127.127.1.0 stratum 10

# client
   tinker panic 0
   server 192.168.122.1