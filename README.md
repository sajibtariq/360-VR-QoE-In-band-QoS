## Predicting XR Services QoE with ML: Insights from In-band Encrypted QoS Features in 360-VR
### Getting Started with the Pre-configured Testbed VM
  - The testbed includes Mininet-Wifi, VR Player, TcpDump, 360 Videos, Viewport Traces, and Network Traces 
   -  **Download link**: [VM](https://drive.google.com/drive/folders/1MhiwJ-_V2TrZsj2xXuprHlFvpN4MzoZH?usp=sharing) [9 GB Size]: Ubuntu 20.04 x64 - **pass: vrexp**
-  **Requirement**: [Oracle  VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- After downloading the vm image "vr-ubuntu 1 1.ova", open VirtualBox, select File --> Import Appliance --> vr-ubuntu 1 1.ova
-  Start the "vr-ubuntu 1" vm from VirtualBox. Select View --> Virtual Screen 1 --> Scale to 100% and select Full-Screen Mode. If the screen does not adjust properly, reboot the vm.
- **Note**: VM has limited storage to store raw data. Thus, we shared a folder from the guest vm to the host machine as follows-
  - Create a folder named *vrexp* in any directory of the host machine which provides enough storage.
  - Suppose the host machine name is *ubuntu*, and  *vrexp* folder is created in "/home/ubuntu/Videos" directory. Now open VirtualBox, select vr-ubuntu 1 --> Settings --> Shared Folders --> Machine Folders, then set the Folder path "/home/ubuntu/Videos/vrexp" and Folder Name "vrexp" and set Auto-mount
  - Start the "vr-ubuntu 1" vm from VirtualBox.  Select and run Devices --> Insert Guest Additions CD Image and reboot the VM
- Locations of the 360 videos, vr player, viewport, and network traces are as follows-
  - *Videos*: /var/www/html/v1, /var/www/html/v2
  - *Viewport traces*: /usr/local/src/per_video
  - *Network traces*: /home/vr-exp2/Traces
  - *VR player* (Adapted from [:arrow_right:](https://github.com/rtcostaf/TOMM2019_VR-EXP)): /home/vr-exp2/player
  
### Data Acquisition
- Open the Ubuntu terminal in the VM and execute the following command:
```bash
 $ cd /home/vr-exp2/mininet-wifi
 $ sudo python3 param.py
```
- Where param.py contains all the parameters information regarding the experiment and calls main.py program each time with a specific set of parameters, the main.py program further calls tc-bw_delay.sh script to set bandwidth parameters from 4G and 5G pre-collected traces using Linux tc.

- The output of each experiment offloads a PCAP file, and VR playout performance log in the corresponding *vrexp* folder.
   - You will see three files regarding VR Playout performance: <file_name>.log, <file_name>-segment.csv, and  <file_name>-session.csv.
   - The <file_name>.log file contains the entire session metrics, such as the total number of tiles downloaded for each zone and resolution, the average bitrate for each zone, total quality switches for each zone, total stall time, and startup delay.
   - The <file_name>-session.csv file contains the same information as <file_name>.log but in comma-separated values (CSV) format. However, there is a minor mistake:
       - The column name "z1_bit" should be replaced with the column name "til_4k_z3"
       - The column name "z2_bit" should be replaced with the column name "z1_bit"
       - The column name "z3_bit" should be replaced with the column name "z2_bit"
       - The column name  "til_4k_z3", should be replaced with the column name "z3_bit" 
   - The <file_name>-segment.csv file contains each downloaded segment's information, such as resolution, data size, download time, bit rate, and so on.

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
- Machine learning model building script  [:arrow_right:](https://github.com/sajibtariq/360-VR-QoE-In-band-QoS/tree/main/Data_Analysis/Machine_Learning)
- **Requirement**:
  - [AutoGluon](https://auto.gluon.ai/stable/install.html)
  - To add AutoGluon in [conda](https://docs.anaconda.com/free/anaconda/install/linux/) environment you may follow [:arrow_right:](https://github.com/autogluon/autogluon/issues/612) [:arrow_right:](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html#installing-non-conda-packages)
  - [Jupyter Notebook](https://jupyter.org/)
  -  NumPy, pandas, seaborn, matplotlib
 
### Standalone VR Client and Content Utilization
- See the given instructions [:arrow_right:](https://docs.google.com/document/d/1IQM-3pIKj_cDVM7pflXPf0Wr4eJIr9WK-q9lZMqeY4g/edit?usp=sharing)

### Citation
We kindly request that you cite our work if you use this project in your research. Please use the following citation format:

M. T. Islam, C. E. Rothenberg and P. H. Gomes, "Predicting XR Services QoE with ML: Insights from In-band Encrypted QoS Features in 360-VR," 2023 IEEE 9th International Conference on Network Softwarization (NetSoft), Madrid, Spain, 2023, pp. 80-88, doi: 10.1109/NetSoft57336.2023.10175481.

```bash
@INPROCEEDINGS{10175481,
  author={Islam, Md Tariqul and Rothenberg, Christian Esteve and Gomes, Pedro Henrique},
  booktitle={2023 IEEE 9th International Conference on Network Softwarization (NetSoft)}, 
  title={Predicting XR Services QoE with ML: Insights from In-band Encrypted QoS Features in 360-VR}, 
  year={2023},
  volume={},
  number={},
  pages={80-88},
  keywords={Solid modeling;Protocols;5G mobile communication;Quality of service;Streaming media;Predictive models;Real-time systems},
  doi={10.1109/NetSoft57336.2023.10175481}}

```
