# Install Docker

In order to connect to a Z-Wave network you need an USB interface, preferably the [Aeotec Z-Stick Gen5](http://aeotec.com/z-wave-usb-stick), that is connected to your PC. Your PC communicates with this device via a serial port. This section describes what to do so that the OpenRemote manager software, which is a containerized (Docker) application, has device access to the serial port on the host. Unfortunately, `Docker for Windows` and `Docker for Mac` do not support device pass through. In case of a Windows or Mac computer you have to install Linux as a virtual machine by means of VirtualBox. There are two options to do this.     

## Windows - Option 1 (Docker Toolbox) 

1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
2. Download and install [VirtualBox Extension Pack](https://www.virtualbox.org/wiki/Downloads)
3. On Windows 10 open the `Windows Features` window and make sure that the following features have **not** been selected: `Hyper-V`, `Virtual Machine Platform`, `Windows Subsystem for Linux`.   
4. Install [Docker Toolbox](http://docs.docker.com/toolbox). Select the following optional components: `Docker Compose for Windows` and `Git for Windows`. Do not select and install `VirtualBox`.
5. Start `Git Bash` and type the following command:
```
docker-machine create --driver virtualbox --virtualbox-boot2docker-url https://github.com/boot2docker/boot2docker/releases/download/v18.06.1-ce/boot2docker.iso default
```
Note that if a virtual machine with the name `default` already exists you can delete it with the following command:
```
docker-machine rm default
```         
6. Start VirtualBox and select the virtual machine with the name `default` on the left side and go to Settings -> Network -> Adapter 1 -> Advanced -> Port Forwarding
7. Add the following rules:

 | Name | Protocol | Host IP | Host Port | Guest IP | Guest Port |
 | --- | --- | --- | --- | --- | --- |
 |postgresql|TCP||5432||5432|
 | keycloak | TCP |  | 8081 |  | 8081 |
 | map | TCP |  | 8082 |  | 8082 |
 | proxy http | TCP |  | 80 |  | 80 |
 | proxy https | TCP |  | 443 |  | 443 |

8. Connect the Aeotec Z-Stick to the PC
9. In VirtualBox go to Settings -> USB
   * Activate `Enable USB Controller`
   * Select `USB 2.0 (EHCI) Controller`
   * Press the `Add USB-Filter` button and select the Aeotec Z-Stick device (Sigma Designs, Inc.)
10. Find out the serial port name. In VirtualBox start the virtual machine with the name `default` on the left side and press the start button. In the boot2docker terminal window type the following command:
```
ls -al /dev/tty* | more
```
In case of a Aeotec Z-Stick Gen5 the USB device name is usually `/dev/ttyACM0`. The older Aeotec Z-Stick S2 has usually the name `/dev/ttyUSB0`.        
11. Open the `docker-compose.yml` file in a text editor and add a `devices:` mapping to the `manager:` service:
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

## Mac - Option 1 (Docker Toolbox) 

1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
2. Download and install [VirtualBox Extension Pack](https://www.virtualbox.org/wiki/Downloads)
3. Install [Docker Toolbox](http://docs.docker.com/toolbox).
4. Start a terminal and execute the following command:
```
docker-machine create --driver virtualbox --virtualbox-boot2docker-url https://github.com/boot2docker/boot2docker/releases/download/v18.06.1-ce/boot2docker.iso default
```
Note that if a virtual machine with the name `default` already exists you can delete it with the following command:
```
docker-machine rm default
```     
5. Start VirtualBox and select the virtual machine with the name `default` on the left side and go to Settings -> Network -> Adapter 1 -> Advanced -> Port Forwarding
6. Add the following rules:

 | Name | Protocol | Host IP | Host Port | Guest IP | Guest Port |
 | --- | --- | --- | --- | --- | --- |
 |postgresql|TCP||5432||5432|
 | keycloak | TCP |  | 8081 |  | 8081 |
 | map | TCP |  | 8082 |  | 8082 |
 | proxy http | TCP |  | 80 |  | 80 |
 | proxy https | TCP |  | 443 |  | 443 |

7. Connect the Aeotec Z-Stick to the PC
8. In VirtualBox go to Settings -> Ports -> USB
   * Activate `Enable USB Controller`
   * Select `USB 2.0 (EHCI) Controller`
   * Press the `Add USB-Filter` button and select the Aeotec Z-Stick device (Sigma Designs, Inc.)
9. Find out the USB device name in the boot2docker virtual machine. In VirtualBox start the virtual machine with the name `default` on the left side and press the start button. In the boot2docker terminal window type the following command:
```
ls -al /dev/tty* | more
```
In case of a Aeotec Z-Stick Gen5 the USB device name is usually `/dev/ttyACM0`. The older Aeotec Z-Stick S2 has usually the name `/dev/ttyUSB0`.        
10. Open the `docker-compose.yml` file in a text editor and add a `devices:` mapping to the `manager:` service:
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

## Windows & Mac - Option 2 (Ubuntu VM)

1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) 
2. Download and install [VirtualBox Extension Pack](https://www.virtualbox.org/wiki/Downloads)     
3. Download [Ubuntu Desktop](https://ubuntu.com/download/desktop)
4. Start VirtualBox and create a new virtual machine by clicking the `New` button on the top toolbar.    
5. Make the following changes as VirtualBox guides you through a wizard:   
   * Type: `Linux`
   * Version: `Ubuntu (64-bit)`
   * Memory size: Minimum `4096 MB`
   * Virtual hard disk size: Minimum `20 GB`
6. Click the `Start` button in the top toolbar and select the Ubuntu iso image that you've downloaded in step #3 in order to install Ubuntu
7. After installing Ubuntu it's recommended to install 'guest additions'. Start the Ubuntu virtual machine and in the upper menu bar select the following: Devices -> Insert Guest Additions CD image... 
8. Install [Docker Engine](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
9. Install [Docker Compose](https://docs.docker.com/compose/install/)
10. Shutdown the Ubuntu virtual machine and connect the Aeotec Z-Stick to the PC. In the VirtualBox Manager go to Settings -> Ports -> USB -> Port1 and add the following configuration:
    * Activate `Enable USB Controller`
    * Select `USB 3.0 (xHCI) Controller`
    * Press the `Add Filter` button and select the Aeotec Z-Stick USB device (Sigma Designs, Inc.) 
11. Find out the serial port name. Run the following command:
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
12. Open the `docker-compose.yml` file in a text editor and add a `devices:` mapping to the `manager:` service:
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

## Linux

1. Install [Docker Engine](https://docs.docker.com/engine/install/)
2. Install [Docker Compose](https://docs.docker.com/compose/install/) 
3. Find out the serial port name. Connect the Aeotec Z-Stick to the PC and execute the following command:
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
4. Open the `docker-compose.yml` file in a text editor and add a `devices:` mapping to the `manager:` service:
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

# Connect a Z-Wave USB Interface

In the following example you link the Z-Wave USB interface that is connected to your PC by using the serial port name, e.g. `/dev/ttyS0`.

1. Login to the manager UI (`https://localhost/master` as `admin/secret`)
2. Select the Assets tab and click `Create asset` at the top of the Asset list on the left 
3. Set the following:
   * Asset name: `ZWave Agent`
   * Parent asset: `Building -> Smart Building` and select 'OK'
   * Asset Type: `Agent`
4. Click `Create asset` at the bottom of the screen
5. Click `Edit asset` and add a new attribute:
   * Name: `ZWave_Interface`
   * Type: `Z-Wave`
6. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) 
7. Edit the serial port name
   * `urn:openremote:protocol:zwave:port`: `/dev/ttyS0` 
7. Click `Save asset` at bottom of the screen

After the Z-Wave USB interface has been successfully linked a full Z-Wave network scan is automatically executed in the background and you are ready to import devices that have already been included to the Z-Wave network.

# Import Z-Wave Devices

1. Select the `ZWave Agent` in the Asset list on the left side
2. Click `Edit asset`
3. Expand the `ZWave_Interface` attribute (using button on the right of the attribute)
4. Click `Select asset` and select the parent asset (e.g. `Building -> Smart Building -> Apartment 3`)
5. Click 'OK' in order to confirm the selection
6. Click `Discover assets`

You can repeat this procedure as often as you want and devices are not imported more than once. You should execute this procedure initially and after you've added a new device to the Z-Wave network.

# Include Z-Wave devices

This procedure describes how to add a new device to the Z-Wave network (Z-Wave device inclusion). 

Note that before you are able to execute this procedure you have to execute the `Import Z-Wave Devices` procedure at least once otherwise the `Z-Wave Controller` asset is missing.

1. Select `Z-Wave Controller` in the asset list on the left side
2. Make sure that `View asset` and `Show live updates` is activated
3. Activate the `Device Inclusion` checkbox and click `Write`   
4. Put the Z-Wave device into inclusion mode (see device manual)
5. Execute the `Import Z-Wave Devices` procedure in order to add the new device to the asset list on the left side

# Exclude Z-Wave devices

This procedure describes how to remove a device from the Z-Wave network (Z-Wave device exclusion). 

Note that before you are able to execute this procedure you have to execute the `Import Z-Wave Devices` procedure at least once otherwise the `Z-Wave Controller` asset is missing.

1. Select `Z-Wave Controller` in the asset list on the left side
2. Make sure that `View asset` and `Show live updates` is activated
3. Activate the `Device Exclusion` checkbox and click `Write`   
4. Put the Z-Wave device into exclusion mode (see device manual)

# Configure Z-Wave device parameters

1. Select the Z-Wave device in the asset list on the left side and and expand it so that you see the `Parameters` group 
2. Expand the `Parameters` group
3. Select the Z-Wave parameter on the left side 
4. Edit the selected Z-Wave parameter on the right side. To get information about the selected Z-Wave parameter read the text in the `Description` attribute on the right side. The attribute with the disabled `Write` button shows the current Z-Wave parameter value. All other attributes with an enabled `Write` button are used to modify the Z-Wave parameter value. In case of a battery powered device you have to wakeup the device (see device manual) so that the configured parameter value can be sent to the device.

# See also

- [[Use UI components|User-Guide:-UI components]]
- [[Demo Smart Building|Demo-Smart-Building]]
- [Get Started](https://openremote.io/get-started-manager/)