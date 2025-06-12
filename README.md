## Introduction
According to [README.md](https://github.com/MoatLab/FEMU/blob/master/README.md), FEMU is a fast, accurate, scalable, and extensible NVMe SSD Emulator.  

Analyzing the structure of a commercial SSD is complex, costly, and involves legal issues.  
For these reasons, in this project, **the operation of the SSD** was analyzed through FEMU and **hot-cold separation** was implemented.


## How is the SSD structured?
Before analyzing how to read and write in FEMU, first, let's explain about the **SSD architecture.**
An SSD is made up of multiple chips. **(Each chip can handle I/O, So, parallel processing is possible using multiple chips, which can improve performance.)**
Also, these chips contain multiple planes.
Multiple planes again contain multiple blocks,
the block is again made up of several pages.
Channel is the bus used to send commands to each chip.


## About the line
<img width="550" alt="line" src="https://github.com/user-attachments/assets/26f5a80e-bc35-4d8b-b8d3-e5f6f41d0dbd" />


## How is I/O handled on an SSD?
<img width="550" alt="io" src="https://github.com/user-attachments/assets/32812b41-f765-4eba-ad53-62977b46ee35" />


## Explanation of various terms
<img width="550" alt="terms" src="https://github.com/user-attachments/assets/03c7bb7c-626b-453a-b62a-be0ab445efb1" />


## A description of the role of each file
- **nvme-io.c**: check for I/O requests originating on the host, it serves to pass request in the submission queue to FTL.  
- **ftl.c**: process the request received with nvme-io.c, and then send it back to nvme-io.c. It serves as the FTL of the SSD. (I/O execution, mapping table management, GC)


## Read, write, GC analysis
<img width="800" alt="analysis" src="https://github.com/user-attachments/assets/d2a04aac-0e73-4fcb-aede-be0ed08a43ec" />


If you want to know more about how read, write, and GC work in FEMU, please click [here](https://github.com/higherlee/FEMU/blob/master/analysis/FEMU-analysis.pdf?raw=true).  
In the performance analysis section, you can also find **write cliﬀ** observations!


## Hot-cold separation
<img width="800" alt="hot_cold" src="https://github.com/user-attachments/assets/2b0c76cc-3306-4bcd-abf9-280bd0c74bd6" />

Currently, hot-cold separation is not implemented in the official FEMU repository.  
I added a statistical module to measure the average IOPS and WAF, and implemented a hot-cold separation through threshold setting. You can also find examples of [hot-cold separation analysis](https://github.com/higherlee/FEMU/blob/master/analysis/performance-analysis.pdf?raw=true) based on various data distributions.


## Additional info
[Visualization](https://github.com/higherlee/FEMU/tree/master/analysis/visualization), [LPN trend source code](https://github.com/higherlee/FEMU/tree/master/analysis/lpn-trend)
