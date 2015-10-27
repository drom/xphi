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

To replicate an optimal operating environment we've made a fresh insallation of Centos 6.7 and  applied all existing updates.

#### Downloaded

[MPSS-3.6](http://registrationcenter.intel.com/irc_nas/8202/mpss-src-3.6.tar)

[Software for Coprocessor OS (k1om)](http://registrationcenter.intel.com/irc_nas/8202/mpss-3.6-k1om.tar)

[Intel Xeon Phi Quick Start Developers Guide MPSS 3.4](https://software.intel.com/sites/default/files/managed/26/d6/Intel_Xeon_Phi_Quick_Start_Developers_Guide-MPSS-3.4.pdf)

###
Installation attempts:
1. 'sudo apt-get install rpm2cpio'
