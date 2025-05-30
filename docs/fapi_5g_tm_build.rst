..    Copyright (c) 2019-2022 Intel
..
..  Licensed under the Apache License, Version 2.0 (the "License");
..  you may not use this file except in compliance with the License.
..  You may obtain a copy of the License at
..
..      http://www.apache.org/licenses/LICENSE-2.0
..
..  Unless required by applicable law or agreed to in writing, software
..  distributed under the License is distributed on an "AS IS" BASIS,
..  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
..  See the License for the specific language governing permissions and
..  limitations under the License.

.. |br| raw:: html

   <br /> 
   
ORAN 5G FAPI TM Installation Guide
==================================

The 5G FAPI TM uses the wls library which uses DPDK as the basis for the shared memory operations 
and requires that DPDK 
be installed in the system since in the makefile it uses the RTE_SDK environment variable when
building the library. |br|
The current release was tested using DPDK version 20.11 but it doesn't preclude the 
use of newer releases. |br|
Also the 5G FAPI TM currently uses the Intel Compiler that is defined as part of the ODULOW documentation.

Contents
--------

- Overview
- Building and Installation
- Command Line Parameters
- Known Issues/Troubleshooting
- License


================================================================================

Overview
--------

This document describes how to install and build the 5G FAPI TM for ODULOW to ODUHIGH
communication as part of the |br|
ORAN Reference Architecture.


================================================================================


Building and Installation
-------------------------

Retrieve the source files from the Linux Foundation Gerrit server:
    `<https://gerrit.o-ran-sc.org/r/gitweb?p=o-du%2Fphy.git;a=summary>`_

1. Make sure that the follwoing environment variables are defined
   DIR_WIRELESS_WLS for the wls_lib and RTE_SDK for the DPDK |br|
2. cd fapi_5g/build |br|
3. $ ./build.sh xclean  // Force full rebuild |br|
4. $ ./build.sh         // Build the 5G FAPI TM |br|

The executable is available at fapi_5g/bin and it is called oran_5g_fapi

Unit Test and validation
---------------------------------

The unit test for the ORAN 5G FAPI TM requires the testmac and L1 binaries that are described
in a later section and that for the O-RAN current Release consists of a suite of tests in timer mode
where the DL, UL and FD paths are exercised for different channel types and numerologies 0, 1 and 2.

1.Open SSH session and cd l1\bin\nr5g\gnb\l1 |br|
2.Issue l1.sh |br|
3.Open a second SSH session and cd fapi_5g\bin |br|
4.Issue ./oran_5g_fapi.sh --cfg oran_5g_fapi.cfg |br|
5.Open a third SSH session and cd l1\bin\nr5g\gnb\testmac |br|
6.Issue ./l2.sh |br|
7.From the testmac command prompt (i.e. the l2 executable) issue::
run Direction Numerology Bandwidth TestCase
where Direction is 0 DL, 1 UL and 2 FD
Numerology 0 15 Khz, 1 30 Khz, 2 60 KHz, etc
Bandwidth is 5, 10 , 20, 100 
Testcase is defined from the set supported in this release
In general issue only the cases provided with this release that have the full set
of supporting files required. |br|
8.Observe in the SSH associated with the testmac the PASS/FAIL status. All of the reference cases
pass.


Testmac cases used for 5g FAPI TM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following DL, UL and PRACH test cases are used for validation.

Downlink Tx Sub6 Test Cases [mu = 0 (15khz) and 5Mhz]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1.Test case 1001 1 PDSCH and 1 Control symbol

2.Test case 1002 1 PUCCH Format 2 channel

Downlink Tx Sub6 Test Cases [mu = 1 (30khz) and 100Mhz]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

3.Test case 1201 4 antennas 1 PDSCH Spatial Multiplexing 144 RBs QAM 16

4.Test case 1204 4 antennas 1 PDSCH Spatial Multiplexing 272 RBs QAM64, DMRS Type 2

5.Test case 1250 SU-MIMO 8 Antennas 4 PDSCH Spatial Multiplexing, 6 D-slots Different RBs per slot

6.Test case 1252 MU-MIMO 16 Antennas 16 PDSCH Spatial Multiplexing, 20 D-Slots Different RBs per slot

Downlink Tx mWave Test Cases [mu=3 (120khz) and 100Mhz]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

7. Test case 1001 2 Antennas 1 PDSCH Spatial Multiplexing, 40 Slots QAM64, 66RBs

8. Test case 1009 2 Antennas 1D and 1S PDSCH Spatial Multiplexing, 160 Slots, QAM64, 66RBs

Uplink Rx Sub6 Test Cases [mu = 0 (15khz) and 5Mhz]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

9.Test case 1001 1 PUSCH

10.Test case 1002 1 PUCCH Format 2

Uplink Rx Sub6 Test Cases [mu = 0 (15khz) and 20Mhz]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

11.Test case 1002 1 PRACH

12.Test case 1003 1 PRACH

Uplink Rx Sub6 Test Cases [mu = 1 (30khz) and 100Mhz]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

13. Test case 1010 1 Antenna, 1 PUSCH Diversity, 1 Slot, QPSK, 272RBs 

14. Test case 1018 2 Antennas, 1 PUSCH Spatial Multiplexing, 20 Slots, QAM256, 144RBs

15. Test case 1086 4 Antennas, 1 PUCCH Format 0, 1 S Slot

16. Test case 1542 MU-MIMO 4 Antennas, 2 PUSCH Spatial Multiplexing, 1 Slot, QAM64, 272 RBs

Uplink Rx mmWave Test Case [mu = 3 (120khz) and 100Mhz]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

17. Test case 1040 2 Antennas, 1 PUSCH Spatial Multiplexing PTRS, QPSK, 1 S slot, 64 RBs

Full Duplex Sub6 Test Case [mu=0 (15khz) and 20Mhz]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

18. Test case 1018 4 Antennas, 4 PDSCH and 8 PDCCH in D Slots and 1 SSB, 4 PUSCH and 58 PUCCH in U Slots Spatial Multiplexing, 40 D slots, 40 U Slots QAM16,16 RBs

Full Duplex Sub6 Test Cases [u = 1 (30khz) and 100Mhz]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

19. Test Case 1300 4 Antennas, 20 Slots, 16 PDSCH {QAM256, mcs28, 272rbs, 12symbols, 4Layers, 16UE/TTI}, 16 PUSCH {QAM64, mcs28, 248rbs, 14symbols, 2Layers, 16UE/TTI}, 16 PDDCH,189 PUCCH and PRACH

20. Test Case 1301 4 Antennas, 20 Slots, 16 PDSCH {QAM64, mcs16, 272rbs, 12symbols, 4Layers, 16UE/TTI}, 16 PUSCH {QAM16, mcs16, 248rbs, 14symbols, 2Layers, 16UE/TTI}, 16 PDSCH, 189 PUCCH.

21. Test Case 1302 4 Antennas, 20 Slots, 16 PDSCH {QAM16, mcs9, 272rbs, 12symbols, 4Layers, 16UE/TTI}, 16 PUSCH {QPSK, mcs9, 248rbs, 14symbols, 2Layers, 16UE/TTI}, 16 PDCCH, 189 PUCCH.

22. Test Case 1303 4 Antennas, 20 Slots, 16 PDSCH {QAM256, mcs28, 190rbs, 12symbols, 4Layers, 16UE/TTI}, 16 PUSCH {QAM64, mcs28, 190rbs, 14symbols, 2Layers, 16UE/TTI}, 16 PDCCH, 189 PUCCH.

23. Test Case 1304 4 Antennas. 20 Slots, 16 PDSCH {QAM64, mcs16, 190rbs, 12symbols, 4Layers, 16UE/TTI}, 16 PUSCH {QAM16, mcs16, 190rbs, 14symbols, 2Layers, 16UE/TTI}, 16 PDCCH, 189 PUCCH.

24. Test Case 1305 4 Antennas, 20 Slots, 16 PDSCH {QAM16, mcs9, 190rbs, 12symbols, 4Layers, 16UE/TTI}, 16 PUSCH {QPSK, mcs9, 190rbs, 14symbols, 2Layers, 16UE/TTI},16 PDCCH, 189 PUCCH.

25. Test Case 1306 4 Antennas, 20 Slots, 16 PDSCH {QAM256, mcs28, 96rbs, 12symbols, 4Layers, 16UE/TTI}, 16 PUSCH {QAM64, mcs28, 96rbs, 14symbols, 2Layers, 16UE/TTI}, 16 PDCCH, 189 PUCCH.

26. Test Case 1307 4 Antennas, 20 Slots, 16 PDSCH {QAM64, mcs16, 96rbs, 12symbols, 4Layers, 16UE/TTI}, 16 PUSCH {QAM16, mcs16, 96rbs, 14symbols, 2Layers, 16UE/TTI}, 16 PDCCH, 189 PUCCH.

27. Test Case 1308 4 Antennas, 20 Slots, 16 PDSCH {QAM16, mcs9, 96rbs, 12symbols, 4Layers, 16UE/TTI}, 16 PUSCH {QPSK, mcs9, 96rbs, 14symbols, 2Layers, 16UE/TTI}, 16 PDCCH, 189 PUCCH.

28. Test Case 1004 2 antennas, 1 Slot, URRLC test case with URLLC in D slot starting at Sym0,3 and in U Slot at sym8,11

29. Test Case 1350 32 Antennas, 20 Slots, 16 PDSCH {QAM256, mcs27, 32rbs,12/10symbols, 4Layers}, 16 PUSCH {QAM64, mcs28, 32rbs, 13 symbols, 2Layers}, 16 PDCCH, 189 PUCCH, PRACH, SRS.

Full Duplex mmWave Test Case [u = 3 (120khz) and 100Mhz]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

30. Test Case 1001 2 Antennas, 80 Slots, 1 PDSCH {QAM64, mcs19, 66rbs, 2Layers}, 1 PUSCH {QAM64, mcs19, 2Layers}, 