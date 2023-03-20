## Predicting XR Services QoE with ML: Insights from In-band Encrypted QoS Features in 360-VR
### Getting Started with the Pre-configured Testbed VM
  - The testbed includes Mininet-Wifi, VR player, TcpDump, 360 Videos, Viewport Traces, and Network Traces 
   -  **Download link**: [VM](https://drive.google.com/file/d/1IDrDLVPnzjDa5cm0AlQTE8b6CHD4WGDS/view?usp=share_link) [9 GB Size]: Ubuntu 20.04 x64 - **pass: vrexp**
-  **Requirement**: [Oracle  VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- After downloading the vm image "vr-ubuntu 1 1.ova", open VirtualBox, select File --> Import Appliance --> vr-ubuntu 1 1.ova
-  Start the "vr-ubuntu 1" vm from VirtualBox. Select View --> Virtual Screen 1 --> Scale to 100% and select Full-Screen Mode. If the screen does not adjust properly, reboot the vm.
- **Note**: VM has limited storage to store raw data. Thus, we shared a folder from the guest vm to the host machine as follows-
  - Create a folder named *vrexp* in any directory of the host machine which provides enough storage.
  - Suppose the host machine name is *ubuntu*, and  *vrexp* folder is created in "/home/ubuntu/Videos" directory. Now open VirtualBox, select vr-ubuntu 1 --> Settings --> Shared Folders --> Machine Folders, then set the Folder path "/home/ubuntu/Videos/vrexp" and Folder Name "vrexp" and set Auto-mount
  - Start the "vr-ubuntu 1" vm from VirtualBox.  Select and run Devices --> Insert Guest Additions CD Image and reboot.
- Locations of the 360 videos, vr player, viewport, and network traces are as follow-
  - *Video*: /var/www/html/v1, /var/www/html/v2
  - *Viewport traces*: /usr/local/bin/src/per_video
  - *Network traces*: /home/vr-exp2/Traces
  - *VR player*: /home/vr-exp2/player
  
### Data Acquisition
- Open the ubuntu terminal in the VM and execute the following command:
```bash
 $ cd /home/vr-exp2/mininet-wifi
 $ sudo python3 param.py
```
- Where param.py contains all the parameters information regarding experiment and calls main.py program each time with a specific set of parameters, the main.py program further calls tc-bw_delay.sh script to set bandwidth parameters from 4G and 5G pre-collected traces using Linux tc.

- The output of each experiment offloads a PCAP file, and VR playout performance log in the corresponding *vrexp* folder.

### Data Preprocess
- ToDo

### Data Analysis
- ToDo
