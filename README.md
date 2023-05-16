## Predicting XR Services QoE with ML: Insights from In-band Encrypted QoS Features in 360-VR
### Getting Started with the Pre-configured Testbed VM
  - The testbed includes Mininet-Wifi, VR Player, TcpDump, 360 Videos, Viewport Traces, and Network Traces 
   -  **Download link**: [VM](https://drive.google.com/file/d/1IDrDLVPnzjDa5cm0AlQTE8b6CHD4WGDS/view?usp=share_link) [9 GB Size]: Ubuntu 20.04 x64 - **pass: vrexp**
-  **Requirement**: [Oracle  VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- After downloading the vm image "vr-ubuntu 1 1.ova", open VirtualBox, select File --> Import Appliance --> vr-ubuntu 1 1.ova
-  Start the "vr-ubuntu 1" vm from VirtualBox. Select View --> Virtual Screen 1 --> Scale to 100% and select Full-Screen Mode. If the screen does not adjust properly, reboot the vm.
- **Note**: VM has limited storage to store raw data. Thus, we shared a folder from the guest vm to the host machine as follows-
  - Create a folder named *vrexp* in any directory of the host machine which provides enough storage.
  - Suppose the host machine name is *ubuntu*, and  *vrexp* folder is created in "/home/ubuntu/Videos" directory. Now open VirtualBox, select vr-ubuntu 1 --> Settings --> Shared Folders --> Machine Folders, then set the Folder path "/home/ubuntu/Videos/vrexp" and Folder Name "vrexp" and set Auto-mount
  - Start the "vr-ubuntu 1" vm from VirtualBox.  Select and run Devices --> Insert Guest Additions CD Image and reboot the vm
- Locations of the 360 videos, vr player, viewport, and network traces are as follow-
  - *Videos*: /var/www/html/v1, /var/www/html/v2
  - *Viewport traces*: /usr/local/bin/src/per_video
  - *Network traces*: /home/vr-exp2/Traces
  - *VR player* (Adpated from [:arrow_right:](https://github.com/rtcostaf/TOMM2019_VR-EXP)): /home/vr-exp2/player
  
### Data Acquisition
- Open the ubuntu terminal in the VM and execute the following command:
```bash
 $ cd /home/vr-exp2/mininet-wifi
 $ sudo python3 param.py
```
- Where param.py contains all the parameters information regarding experiment and calls main.py program each time with a specific set of parameters, the main.py program further calls tc-bw_delay.sh script to set bandwidth parameters from 4G and 5G pre-collected traces using Linux tc.

- The output of each experiment offloads a PCAP file, and VR playout performance log in the corresponding *vrexp* folder.

### Data Preprocess
- Raw data sample, including PCAP and VR performance metrics log file  [:arrow_right:](https://github.com/sajibtariq/360-VR-QoE-In-band-QoS/tree/main/Data_Preprocess/Raw_Data_Sample)
- QoE value measurement script  [:arrow_right:](https://github.com/sajibtariq/360-VR-QoE-In-band-QoS/tree/main/Data_Preprocess/QoE_Value_Calculation) and output CSV file containing QoE metrics  [:arrow_right: an example](https://github.com/sajibtariq/360-VR-QoE-In-band-QoS/blob/main/Data_Preprocess/Raw_Data_Sample/HTTPS(TCP)/HTTP-1.1/Persistant/host-1_ts-60_thd-1_vpe-0_algo-0_bft-6_delay-5/host-1_ts-60_thd-1_vpe-0_algo-0_bft-6_delay-5-session1_new.csv)
- QoS feature extraction script [:arrow_right:](https://github.com/sajibtariq/360-VR-QoE-In-band-QoS/tree/main/Data_Preprocess/QoS_Feature_Calculation) and output CSV file containing all QoS features  [:arrow_right: an example](https://github.com/sajibtariq/360-VR-QoE-In-band-QoS/blob/main/Data_Preprocess/Raw_Data_Sample/HTTPS(TCP)/HTTP-1.1/Persistant/host-1_ts-60_thd-1_vpe-0_algo-0_bft-6_delay-5/host-1_ts-60_thd-1_vpe-0_algo-0_bft-6_delay-5-pcap.csv)
- QoE metrics and QoS features merge script [:arrow_right:](https://github.com/sajibtariq/360-VR-QoE-In-band-QoS/tree/main/Data_Preprocess/QoE_QoS_Merge), with output CSV file as the final dataset sample  [:arrow_right:](https://github.com/sajibtariq/360-VR-QoE-In-band-QoS/tree/main/Data_Preprocess/Final_Dataset_Sample)
- **Requirement**:
  - [Scapy](https://scapy.net/)- to read pcap
  - [Jupyter Notebook](https://jupyter.org/)
  - NumPy, pandas  



### Data Analysis
- Machine learning model building script  [:arrow_right:]([https://github.com/sajibtariq/360-VR-QoE-In-band-QoS/tree/main/Data_Preprocess/Raw_Data_Sample](https://github.com/sajibtariq/360-VR-QoE-In-band-QoS/tree/main/Data_Analysis/Machine_Learning))
- **Requirement**:
  - [AutoGluon](https://auto.gluon.ai/stable/install.html)
  - To add AutoGluon in [conda](https://docs.anaconda.com/free/anaconda/install/linux/) environment you may follow [:arrow_right:](https://github.com/autogluon/autogluon/issues/612) [:arrow_right:](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html#installing-non-conda-packages)
  - [Jupyter Notebook](https://jupyter.org/)
  -  NumPy, pandas, seaborn, matplotlib 
