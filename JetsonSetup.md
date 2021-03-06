# 

## Table of Contents
[JetPack on Nvidia DevKit](#jetpack-on-nvidia-devkit)  
[JetPack on ConnectTech Orbitty Carrier](#jetpack-on-connecttech-orbitty-carrier)  
[CUDA + PyTorch](#cuda-and-pytorch)  
[ROS](#ros)  
[ROS Melodic With Python3](#ros-melodic-with-python3)  
[Optional Tools](#optional-tools)  

You will require an xUbuntu 18.04 computer to run the [Nvidia SDK Manager](https://developer.nvidia.com/nvidia-sdk-manager). As of the time this documentation was written, `sdkmanager` is incompatible with 20.04, but it can be run under a [VMWare Workstation Player](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html) virtual machine running 18.04 - you just need to [connect the Jetson USB device to the VM](https://www.vmware.com/support/ws5/doc/ws_devices_usb_connect.html)

## JetPack on Nvidia DevKit
1. Run `sdkmanager` on the host computer, and select both "Host Machine" and "Jetson TX2" to download JetPack to the host.
    ![image](https://user-images.githubusercontent.com/3406269/116788542-c9e12780-aa6f-11eb-94ee-167d64f6a90e.png)
    Select CUDA and additional packages as desired
    Click next past Step 02, accepting the license along the way.
2. After Step 03 of the sdkmanager, the Jetpack image will be created, and it will ask you to connect the Jetson to the host computer: 
    ![image](https://user-images.githubusercontent.com/3406269/116788518-a5854b00-aa6f-11eb-9d03-fb108bf09b9e.png)
3. Connect the devkit using the microUSB OTG port on the devkit and enter recovery mode:
    1. Plug in the power cable to the devkit board, but do not power on yet.
    2. Press and hold the `REC` (Recovery) button
    3. Press and release the `POWERBTN` (Power) button
    4. Release the `REC` button
4. Select "Manual Setup" in the dropdown option list on the sdkmanager window, and click on the "Flash" button
5. After JetPack is installed, connect the devkit to a monitor, keyboard and mouse, and complete the Ubuntu setup wizard.
6. If you selected CUDA and additional packages in step 1, it will ask you for the IP address and login details of the devkit. Continue on to install [CUDA + PyTorch](cuda-and-pytorch)  


## JetPack on ConnectTech Orbitty Carrier

1. Do **not** connect the Jetson to your computer at this stage.
2. Run `sdkmanager` on the host computer, and select both "Host Machine" and "Jetson TX2" to download JetPack to the host.
    ![image](https://user-images.githubusercontent.com/3406269/116788542-c9e12780-aa6f-11eb-94ee-167d64f6a90e.png)
    Click next past Step 02, accepting the license along the way.
3. After Step 03 of the sdkmanager, the Jetpack image will be created, and it will ask you to connect the Jetson to the host computer: 
    ![image](https://user-images.githubusercontent.com/3406269/116788518-a5854b00-aa6f-11eb-9d03-fb108bf09b9e.png)
4. Click on "Skip" and confirm that you want to skip installation to the Jetson:
    ![image](https://user-images.githubusercontent.com/3406269/116788576-03b22e00-aa70-11eb-8b3f-8ebebefb6994.png)
5. At this point, you should see JetPack installed on your host computer at `~/nvidia/nvidia_sdk/`:
    ```bash
    joydeepb@ubuntu:~$ cd nvidia/nvidia_sdk
    joydeepb@ubuntu:~/nvidia/nvidia_sdk$ ls -lthr
    total 8.0K
    drwxrwxr-x 4 joydeepb joydeepb 4.0K May  1 09:16 JetPack_4.5.1_Linux
    drwxrwxr-x 3 joydeepb joydeepb 4.0K May  1 09:19 JetPack_4.5.1_Linux_JETSON_TX2
    ```
6. Go to the [Connect Tech support page](http://connecttech.com/product/orbitty-carrier-for-nvidia-jetson-tx2-tx1/) and download 
    the L4T support package into `<JetPack_install_dir>/JetPack_4.5.1_Linux_JETSON_TX2/Linux_for_Tegra/`. For example:
   ```bash
    joydeepb@ubuntu:~$ cd nvidia/nvidia_sdk/JetPack_4.5.1_Linux_JETSON_TX2/Linux_for_Tegra/
    joydeepb@ubuntu:~/nvidia/nvidia_sdk/JetPack_4.5.1_Linux_JETSON_TX2/Linux_for_Tegra$ wget https://connecttech.com/ftp/Drivers/CTI-L4T-TX2-32.5-V001.tgz
    --2021-05-01 09:14:59--  https://connecttech.com/ftp/Drivers/CTI-L4T-TX2-32.5-V001.tgz
    Resolving connecttech.com (connecttech.com)... 104.26.6.94, 172.67.72.40, 104.26.7.94, ...
    Connecting to connecttech.com (connecttech.com)|104.26.6.94|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 519331075 (495M) [application/octet-stream]
    Saving to: ‘CTI-L4T-TX2-32.5-V001.tgz’

    CTI-L4T-TX2-32.5-V001.tgz                                 100%[===================================================================================================================================>] 495.27M  15.5MB/s    in 32s     

    2021-05-01 09:15:31 (15.4 MB/s) - ‘CTI-L4T-TX2-32.5-V001.tgz’ saved [519331075/519331075]
   ```
1. Extract the archive, and run the install script in the extracted directory:
    ```
    joydeepb@ubuntu:~/nvidia/nvidia_sdk/JetPack_4.5.1_Linux_JETSON_TX2/Linux_for_Tegra$ tar xzf CTI-L4T-TX2-32.5-V001.tgz
    joydeepb@ubuntu:~/nvidia/nvidia_sdk/JetPack_4.5.1_Linux_JETSON_TX2/Linux_for_Tegra$ cd ./CTI-L4T
    joydeepb@ubuntu:~/nvidia/nvidia_sdk/JetPack_4.5.1_Linux_JETSON_TX2/Linux_for_Tegra/CTI-L4T$ sudo ./install.sh
    ```
1. Connect the Jetson to the host computer and enter firmware update mode:
      1. Connect the USB OTG port
      2. Press and hold the recovery button
      3. Press and release the power button
      4. Release the recovery button
3. Now reflash the Jetson from the `Linux_for_Tegra` directory:
    ```
    joydeepb@ubuntu:~/nvidia/nvidia_sdk/JetPack_4.5.1_Linux_JETSON_TX2/Linux_for_Tegra$ sudo ./cti-flash.sh
    ```

## CUDA and PyTorch

1. Ensure that the Jetson is running, booted into Ubuntu, and accessible via the local network (e.g. WiFi) 
    from the host computer. Let the Jetson's IP address on the local network be `192.168.10.100`
2. On the host computer, run `sdkmanager`
3. Select "Jetson" as the product category, deselect Host Machine, Jetson TX2 as the Target Hardware, and 
    select the correct JetPack version installed on the Jetson.
4. On step 2, **deselect "Jetson OS"** from the list of components and select `CUDA` and additional components:
    ![image](https://user-images.githubusercontent.com/3406269/116788926-dfefe780-aa71-11eb-8396-6ba00247c308.png)
5. Manually enter the IP address of the Jetson computer, and the admin username and password:
    ![image](https://user-images.githubusercontent.com/3406269/116788939-f9912f00-aa71-11eb-980d-24dc96409fd3.png)
6. To install pytorch, follow the instructions here: https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-8-0-now-available/72048 

## ROS

Install ROS Melodic as per the installation instructions: http://wiki.ros.org/melodic/Installation/Ubuntu

## ROS Melodic With Python3

After installing ROS, install rospkg for python3:
```
sudo apt install python3-pip python3-all-dev python3-rospkg
```
This will prompt to install python3-rospkg and to remove ROS packages (already installed). Select Yes for that prompt. 
This will remove ROS packages and we will have to re-install them.
```
sudo apt install ros-melodic-desktop-full --fix-missing
```
After this, you should be able to run PyTorch with Cuda alongside ROS using Python3.

## Optional Tools

To monitor CPU and GPU usage, clone and install [Jetson Stats](https://github.com/rbonghi/jetson_stats)
![image](https://github.com/rbonghi/jetson_stats/wiki/images/jtop.gif)
