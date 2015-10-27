We have a host system with the ASUS P9X79-WS motherboard.

For successful booting the BIOS should be configured to support "memory mapped I/O address ranges above 4GB".

[look here for details]
(https://www.pugetsystems.com/labs/articles/Will-your-motherboard-work-with-Intel-Xeon-Phi-490/ f)

After booting the `lspci | grep Phi` command yelds the following:

``` 
>>lspci | grep Phi
02:00.0 Co-processor: Intel Corporation Xeon Phi coprocessor 5100 series (rev 20)
```
or more detailed report:
```
>>lspci -s 02:00 -vv
02:00.0 Co-processor: Intel Corporation Xeon Phi coprocessor 5100 series (rev 20)
        Subsystem: Intel Corporation Device d804                                 
        Control: I/O+ Mem+ BusMaster+ SpecCycle- MemWINV- VGASnoop- ParErr- Stepping- SERR- FastB2B- DisINTx+
        Status: Cap+ 66MHz- UDF- FastB2B- ParErr- DEVSEL=fast >TAbort- <TAbort- <MAbort- >SERR- <PERR- INTx- 
        Latency: 0, Cache Line Size: 64 bytes
        Interrupt: pin A routed to IRQ 40
        Region 0: Memory at 380c00000000 (64-bit, prefetchable) [size=8G]
        Region 4: Memory at fb600000 (64-bit, non-prefetchable) [size=128K]
        Capabilities: [44] Power Management version 3
                Flags: PMEClk- DSI- D1- D2- AuxCurrent=0mA PME(D0+,D1-,D2-,D3hot-,D3cold-)
                Status: D0 NoSoftRst+ PME-Enable- DSel=0 DScale=0 PME-
        Capabilities: [4c] Express (v2) Endpoint, MSI 00
                DevCap: MaxPayload 256 bytes, PhantFunc 0, Latency L0s <4us, L1 <64us
                        ExtTag+ AttnBtn- AttnInd- PwrInd- RBE+ FLReset-
                DevCtl: Report errors: Correctable- Non-Fatal- Fatal- Unsupported-
                        RlxdOrd- ExtTag- PhantFunc- AuxPwr- NoSnoop+
                        MaxPayload 256 bytes, MaxReadReq 512 bytes
                DevSta: CorrErr- UncorrErr- FatalErr- UnsuppReq- AuxPwr- TransPend-
                LnkCap: Port #0, Speed 5GT/s, Width x16, ASPM L0s L1, Exit Latency L0s <4us, L1 unlimited
                        ClockPM- Surprise- LLActRep- BwNot-
                LnkCtl: ASPM Disabled; RCB 64 bytes Disabled- CommClk+
                        ExtSynch- ClockPM- AutWidDis- BWInt- AutBWInt-
                LnkSta: Speed 5GT/s, Width x16, TrErr- Train- SlotClk+ DLActive- BWMgmt- ABWMgmt-
                DevCap2: Completion Timeout: Range AB, TimeoutDis+, LTR-, OBFF Not Supported
                DevCtl2: Completion Timeout: 50us to 50ms, TimeoutDis-, LTR-, OBFF Disabled
                LnkCtl2: Target Link Speed: 5GT/s, EnterCompliance- SpeedDis-
                         Transmit Margin: Normal Operating Range, EnterModifiedCompliance- ComplianceSOS-
                         Compliance De-emphasis: -6dB
                LnkSta2: Current De-emphasis Level: -6dB, EqualizationComplete-, EqualizationPhase1-
                         EqualizationPhase2-, EqualizationPhase3-, LinkEqualizationRequest-
        Capabilities: [88] MSI: Enable- Count=1/16 Maskable- 64bit+
                Address: 0000000000000000  Data: 0000
        Capabilities: [98] MSI-X: Enable+ Count=16 Masked-
                Vector table: BAR=4 offset=00017000
                PBA: BAR=4 offset=00018000
        Capabilities: [100 v1] Advanced Error Reporting
                UESta:  DLP- SDES- TLP- FCP- CmpltTO- CmpltAbrt- UnxCmplt- RxOF- MalfTLP- ECRC- UnsupReq- ACSViol-
                UEMsk:  DLP- SDES- TLP- FCP- CmpltTO- CmpltAbrt- UnxCmplt- RxOF- MalfTLP- ECRC- UnsupReq- ACSViol-
                UESvrt: DLP+ SDES- TLP- FCP+ CmpltTO- CmpltAbrt- UnxCmplt- RxOF+ MalfTLP+ ECRC- UnsupReq- ACSViol-
                CESta:  RxErr- BadTLP- BadDLLP- Rollover- Timeout- NonFatalErr-
                CEMsk:  RxErr- BadTLP- BadDLLP- Rollover- Timeout- NonFatalErr+
                AERCap: First Error Pointer: 00, GenCap- CGenEn- ChkCap- ChkEn-
        Kernel driver in use: mic
```
#### OS Installation

To replicate an optimal operating environment we've made a fresh insallation of Centos 6.7 and  applied all existing updates. The kernel version (`uname -r`) is 2.6.32-573.7.1.el6.x86_64, which is listed in the Intel documentation [readme](http://registrationcenter.intel.com/irc_nas/8202/readme.txt)as compatible.

#### Downloaded

[MPSS-3.6](http://registrationcenter.intel.com/irc_nas/8202/mpss-src-3.6.tar)

[Software for Coprocessor OS (k1om)](http://registrationcenter.intel.com/irc_nas/8202/mpss-3.6-k1om.tar)

[Intel Xeon Phi Quick Start Developers Guide MPSS 3.4](https://software.intel.com/sites/default/files/managed/26/d6/Intel_Xeon_Phi_Quick_Start_Developers_Guide-MPSS-3.4.pdf)

#### Installation sequence:
Generally following the instructions in the [readme](http://registrationcenter.intel.com/irc_nas/8202/readme.txt)a results in an apparently successful installation with just a few irregularities that are listed below:

```
[root@NEUTRINO mpss-3.6]# modprobe mic

Message from syslogd@NEUTRINO at Oct 27 00:50:30 ...
 kernel:BUG: soft lockup - CPU#1 stuck for 67s! [modprobe:6610]
```

```
[root@NEUTRINO mpss-3.6]# micctrl --initdefaults
[Warning] mic0: Generating compatibility network config file /opt/intel/mic/filesystem/mic0/etc/sysconfig/network/ifcfg-mic0 for IDB.
[Warning]       This may be problematic at best and will be removed in a future release, Check with the IDB release.
[root@NEUTRINO mpss-3.6]# micctrl -s
mic0: ready
```

```
[root@NEUTRINO mpss-3.6]# /usr/bin/micflash -update -device all
No image path specified - Searching: /usr/share/mpss/flash
mic0: Flash image: /usr/share/mpss/flash/EXT_HP2_C0_0391-02.rom.smc
mic0: Flash update started
mic0: Flash update done
mic0: SMC update started
micflash: mic0: Flash operation timed out due to a corrupted image file or an internal error.

mic0: Transitioning to ready state

Please restart host for flash changes to take effect
```

#### Accessing the Co-processor

Starting the mpss service is done via 

```
[root@NEUTRINO master]# service mpss start
Loading MIC module:                                        [  OK  ]
Starting Intel(R) MPSS:                                    [  OK  ]
mic0: online (mode: linux image: /usr/share/mpss/boot/bzImage-knightscorner)
```

Manipulations with the Xeon Phi card are done via ssh layer that runs over the PCIe physical interface with the help of a virtual network driver:  `ssh mic0`

There is a file system "inside the co-processor":

```
[root@NEUTRINO-mic0 ~]# ls -all /                                                                                  
drwxr-xr-x   17 root     root           380 Dec 31  1969 .                                                         
drwxr-xr-x   17 root     root           380 Dec 31  1969 ..                                                        
drwxr-xr-x    2 root     root          1540 Dec 31  1969 bin                                                       
drwxr-xr-x    2 root     root            40 Oct  1 04:01 boot                                                      
drwxr-xr-x   12 root     root          2500 Oct  1 04:50 dev                                                       
drwxr-xr-x   29 root     root          1520 Oct  1 04:50 etc                                                       
drwxr-xr-x    5 root     root           100 Dec 31  1969 home                                                      
-rwxr-xr-x    1 root     root          5587 Oct  1 04:50 init                                                      
drwxr-xr-x    3 root     root            60 Dec 31  1969 lib                                                       
drwxr-xr-x    4 root     root          1500 Dec 31  1969 lib64                                                     
drwxr-xr-x   10 root     root           200 Dec 31  1969 media                                                     
drwxr-xr-x    2 root     root           120 Dec 31  1969 mnt                                                       
dr-xr-xr-x 1027 root     root             0 Dec 31  1969 proc                                                      
drwx------    2 root     root            60 Dec 31  1969 root                                                      
drwxr-xr-x    2 root     root          1640 Dec 31  1969 sbin                                                      
drwxr-xr-x   12 root     root             0 Dec 31  1969 sys                                                       
lrwxrwxrwx    1 root     root             8 Dec 31  1969 tmp -> /var/tmp                                           
drwxr-xr-x   10 root     root           200 Dec 31  1969 usr                                                       
drwxr-xr-x    8 root     root           260 Dec 31  1969 var                                                       
```
Cpuinfo is *super* verbose returning 240 (!!!) identical processor descriptions:

```
[root@NEUTRINO-mic0 ~]# cat /proc/cpuinfo                                                                          
processor       : 0                                                                                                
...
processor       : 239
vendor_id       : GenuineIntel
cpu family      : 11          
model           : 1           
model name      : 0b/01       
stepping        : 2           
cpu MHz         : 1052.630    
cache size      : 512 KB      
physical id     : 0           
siblings        : 240         
core id         : 59          
cpu cores       : 60          
apicid          : 239         
initial apicid  : 239         
fpu             : yes         
fpu_exception   : yes         
cpuid level     : 4           
wp              : yes         
flags           : fpu vme de pse tsc msr pae mce cx8 apic mtrr mca pat fxsr ht syscall nx lm nopl lahf_lm
bogomips        : 2111.12                                                                                
clflush size    : 64                                                                                     
cache_alignment : 64                                                                                     
address sizes   : 40 bits physical, 48 bits virtual                                                      
power management:                                                                                        

```

Memory as one would have expected totals to 8 GB:

```
[root@NEUTRINO-mic0 ~]# cat /proc/meminfo 
MemTotal:        7882356 kB               
MemFree:         7609592 kB
Buffers:               0 kB
Cached:            56152 kB
SwapCached:            0 kB
Active:            12352 kB
Inactive:          46448 kB
Active(anon):      12352 kB
Inactive(anon):    46448 kB
Active(file):          0 kB
Inactive(file):        0 kB
Unevictable:           0 kB
Mlocked:               0 kB
SwapTotal:             0 kB
SwapFree:              0 kB
Dirty:                 0 kB
Writeback:             0 kB
AnonPages:          2804 kB
Mapped:             4256 kB
Shmem:             56152 kB
Slab:              93992 kB
SReclaimable:      20624 kB
SUnreclaim:        73368 kB
KernelStack:       10048 kB
PageTables:          604 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     3941176 kB
Committed_AS:      61664 kB
VmallocTotal:   34359738367 kB
VmallocUsed:       32204 kB
VmallocChunk:   34359702460 kB
AnonHugePages:         0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:       10236 kB
DirectMap2M:     8097792 kB
```

In the idle mode there is not much happening inside the co-processor:

```
[root@NEUTRINO-mic0 ~]# top               
top - 01:12:05 up 10 min,  1 user,  load average: 0.02, 0.03, 0.02
Tasks: 1015 total,   1 running, 1014 sleeping,   0 stopped,   0 zombie
Cpu(s):  0.0%us,  0.1%sy,  0.0%ni, 99.9%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
Mem:   7882356k total,   274908k used,  7607448k free,        0k buffers
Swap:        0k total,        0k used,        0k free,    56152k cached

   PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND                                                                                
  5061 root      20   0 13376 1868  852 R    5  0.0   0:00.73 top                                                                                    
  3492 root      20   0     0    0    0 S    0  0.0   0:00.01 kworker/175:1                                                                          
  4334 root      20   0     0    0    0 S    0  0.0   0:00.01 kworker/95:1                                                                           
  4476 root      20   0     0    0    0 S    0  0.0   0:00.01 kworker/115:1                                                                          
  4478 root      20   0     0    0    0 S    0  0.0   0:00.01 kworker/183:1                                                                          
  4479 root      20   0     0    0    0 S    0  0.0   0:00.01 kworker/151:1                                                                          
  4503 root      20   0     0    0    0 S    0  0.0   0:00.01 kworker/179:1                                                                          
  4516 root      20   0     0    0    0 S    0  0.0   0:00.01 kworker/80:1                                                                           
  5050 root      20   0 46164 2864 2276 S    0  0.0   0:00.18 sshd                                                                                   
  5058 root      20   0     0    0    0 S    0  0.0   0:00.15 kworker/u:8                                                                            
     1 root      20   0  3892  604  508 S    0  0.0   0:04.08 init                                                                                   
     2 root      20   0     0    0    0 S    0  0.0   0:00.09 kthreadd                                                                               
     3 root      20   0     0    0    0 S    0  0.0   0:00.00 ksoftirqd/0                                                                            
     4 root      20   0     0    0    0 S    0  0.0   0:00.00 kworker/0:0                                                                            
     5 root      20   0     0    0    0 S    0  0.0   0:00.74 kworker/u:0                                                                            
     6 root      RT   0     0    0    0 S    0  0.0   0:00.00 migration/0                                                                            
     7 root      RT   0     0    0    0 S    0  0.0   0:00.00 migration/1                                                                            
     8 root      20   0     0    0    0 S    0  0.0   0:00.00 kworker/1:0                                                                            
     9 root      20   0     0    0    0 S    0  0.0   0:00.00 ksoftirqd/1                                                                            
    10 root      20   0     0    0    0 S    0  0.0   0:00.03 kworker/0:1                                                                            
    11 root      RT   0     0    0    0 S    0  0.0   0:00.00 migration/2                                                                            
    12 root      20   0     0    0    0 S    0  0.0   0:00.00 kworker/2:0                                                                            
    13 root      20   0     0    0    0 S    0  0.0   0:00.03 ksoftirqd/2                                                                            
    14 root      RT   0     0    0    0 S    0  0.0   0:00.00 migration/3                                                                            
    15 root      20   0     0    0    0 S    0  0.0   0:00.00 kworker/3:0                                                                            
    16 root      20   0     0    0    0 S    0  0.0   0:00.00 ksoftirqd/3                                                                            
    17 root      RT   0     0    0    0 S    0  0.0   0:00.00 migration/4                                                                            
    18 root      20   0     0    0    0 S    0  0.0   0:00.00 kworker/4:0                                                                            
    19 root      20   0     0    0    0 S    0  0.0   0:00.00 ksoftirqd/4                                                                            
    20 root      RT   0     0    0    0 S    0  0.0   0:00.00 migration/5                                                                            
    21 root      20   0     0    0    0 S    0  0.0   0:00.00 kworker/5:0                                                                            
    22 root      20   0     0    0    0 S    0  0.0   0:00.00 ksoftirqd/5                                                                            
    23 root      RT   0     0    0    0 S    0  0.0   0:00.00 migration/6                                                                            
```
