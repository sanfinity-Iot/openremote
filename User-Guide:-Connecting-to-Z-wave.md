# Configure Serial Port

In order to connect to a Z-Wave network you need an USB interface, preferably the [Aeotec Z-Stick Gen5](http://aeotec.com/z-wave-usb-stick), that is attached to your PC. Your PC communicates with this device via a serial port. First of all you have to determine which serial port was assigned to the Z-Wave USB Stick on the Docker host. After that your are able to configure a `docker-compose` device mapping so that the serial port is accessible within the docker container (device passthrough). Unfortunately, `Docker for Windows` and `Docker for Mac` do not support device pass through. In this case you have to resort to the legacy Docker Toolbox software.     

## Linux Serial Port 

1. Connect the Aeotec Z-Stick to the PC
2. Run the dmesg command
```
dmesg -w
```
You should see something like the following:
```
[15624.940828] cdc_acm 2-1:1.0: ttyACM0: USB ACM device
```
In this example the resulting serial port name would be:
```
/dev/ttyACM0
``` 
3. Open the `docker-compose.yml` file in a text editor and add a `devices:` mapping to the `manager:` service:
```yml
manager:
  restart: always
  extends:
    file: profile/deploy.yml
    service: manager
  depends_on:
    keycloak:
      condition: service_healthy
  volumes:
    - deployment-data:/deployment
    - zwave-data:/zwave
  devices:
    - /dev/ttyACM0:/dev/ttyS0
```
 
## Windows Serial Port 

1. Install [Docker Toolbox](http://docs.docker.com/toolbox)
2. Open VirtualBox (start VirtualBox.exe) and select the virtual machine with the name `default` on the left side and go to Settings -> Network -> Adapter 1 -> Advanced -> Port Forwarding
4. Add the following rules:
 
 | Name | Protocol | Host IP | Host Port | Guest IP | Guest Port |
 | --- | --- | --- | --- | --- | --- |
 |postgresql|TCP||5432||5432|
 | keycloak | TCP |  | 8081 |  | 8081 |
 | map | TCP |  | 8082 |  | 8082 |
 | proxy http | TCP |  | 80 |  | 80 |
 | proxy https | TCP |  | 443 |  | 443 |

5. Connect the Aeotec Z-Stick to the PC
6. Start the Windows Device Manager and check which serial port was assigned to the device:
 Control Panel -> Device Manager -> Ports. The serial port name is something like COM1, COM2, COM3...  
7. In VirtualBox go to Settings -> Ports -> Serial Ports -> Port 1
   * Activate `Enable Serial Port`
   * Select `Port Number`: `COM1
   * Select `Port Mode`: `Host Device`
   * Edit `Path/Address` and insert the serial port name from the previous step (e.g. COM3)  
8. Open the `docker-compose.yml` file in a text editor and add a `devices:` mapping to the `manager:` service:
```yml
manager:
  restart: always
  extends:
    file: profile/deploy.yml
    service: manager
  depends_on:
    keycloak:
      condition: service_healthy
  volumes:
    - deployment-data:/deployment
    - zwave-data:/zwave
  devices:
    - /dev/ttyS0:/dev/ttyS0
```
 
## Mac Serial Port 

1. Install [Docker Toolbox](http://docs.docker.com/toolbox)
2. Open VirtualBox (start VirtualBox.app) and select the virtual machine with the name `default` on the left side and go to Settings -> Network -> Adapter 1 -> Advanced -> Port Forwarding
4. Add the following rules:
 
 | Name | Protocol | Host IP | Host Port | Guest IP | Guest Port |
 | --- | --- | --- | --- | --- | --- |
 |postgresql|TCP||5432||5432|
 | keycloak | TCP |  | 8081 |  | 8081 |
 | map | TCP |  | 8082 |  | 8082 |
 | proxy http | TCP |  | 80 |  | 80 |
 | proxy https | TCP |  | 443 |  | 443 |

5. Connect the Aeotec Z-Stick to the Mac 
6. Open the Terminal app and execute the following command to determine the serial port associated with the device:
```
ls -l /dev/cu.*
```
Output in case of Aeotec Z-Stick Gen5:
```
crw-rw-rw-  1 root  wheel   18,  17 Dec  6 13:45 /dev/cu.usbmodem14201
```
Output in case of Aeotec Z-Stick S2:
```
crw-rw-rw-  1 root  wheel   18,  19 Dec  6 13:51 /dev/cu.SLAB_USBtoUART 
```
7. In VirtualBox go to Settings -> Ports -> Serial Ports -> Port 1
   * Activate `Enable Serial Port`
   * Select `Port Number`: `COM1`
   * Select `Port Mode`: `Host Device`
   * Edit `Path/Address` and insert the serial port name from the previous step (e.g. /dev/cu.usbmodem14201)  
8. Open the `docker-compose.yml` file in a text editor and add a `devices:` mapping to the `manager:` service:
```yml
manager:
  restart: always
  extends:
    file: profile/deploy.yml
    service: manager
  depends_on:
    keycloak:
      condition: service_healthy
  volumes:
    - deployment-data:/deployment
    - zwave-data:/zwave
  devices:
    - /dev/ttyS0:/dev/ttyS0
```

T.B.D.

# See also

- [[Use UI components|User-Guide:-UI components]]
- [[Demo Smart Building|Demo-Smart-Building]]
- [Get Started](https://openremote.io/get-started-manager/)