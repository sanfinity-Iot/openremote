# Configure Serial Port

In order to connect to a Z-Wave network you need an USB interface, preferably the [Aeotec Z-Stick Gen5](http://aeotec.com/z-wave-usb-stick), that is connected to your PC. Your PC communicates with this device via a serial port. This section describes what to do so that the OpenRemote manager software, which is a containerized (Docker) application, has device access to the serial port on the host. Unfortunately, `Docker for Windows` and `Docker for Mac` do not support device pass through. In case of a Windows or Mac computer you have to install Linux as a virtual machine by means of VirtualBox.     

## Install Linux on Windows & Mac

1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)      
2. Download [Ubuntu Desktop] (https://ubuntu.com/download/desktop)
3. Start VirtualBox and create a new virtual machine by clicking the `New` button on the top toolbar.    
4. Make the following changes as VirtualBox guides you through a wizard:   
   * Type: `Linux`
   * Version: `Ubuntu (64-bit)`
   * Memory size: Minimum `4096 MB`
5. Click the `Start` button in the top toolbar and select the Ubuntu iso image that you've downloaded in step #2 in order to install Ubuntu
6. After installing Ubuntu it's recommended to install 'guest additions'. Start the Ubuntu virtual machine and in the upper menu bar select the following: Devices -> Insert Guest Additions CD image... 
7. Install [Docker Engine - Community for Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
8. Install [Docker Compose](https://docs.docker.com/compose/install/)
9. Install Git: `sudo apt install git`
10. Shutdown the Ubuntu virtual machine and connect the Aeotec Z-Stick to the PC. In the VirtualBox Manager go to Settings -> Ports -> USB -> Port1 and add the following configuration:
   * Activate `Enable USB Controller`
   * Select `USB 2.0 (EHCI) Controller`
   * Press the `Add Filter` button and select the Aeotec Z-Stick USB device (Sigma Designs, Inc.) 

## Configure Docker serial port 

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

# See also

- [[Use UI components|User-Guide:-UI components]]
- [[Demo Smart Building|Demo-Smart-Building]]
- [Get Started](https://openremote.io/get-started-manager/)