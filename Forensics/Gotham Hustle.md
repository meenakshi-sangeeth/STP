## Challenge Description 

Gotham’s underbelly trembles as whispers spread—The Riddler’s back, leaving cryptic puzzles across the city’s darkest corners. Every clue is a trap, every answer another step into madness. Think you can outsmart him? Step into Gotham’s shadows and prove it. Let the Batman's Hustle get its recognition!

## Solution

Tried all the basic forensic tools and then proceeded to use volatility

```bash
volatility -f gotham.raw imageinfo
```

`Win7SP1x64` was listed in the output so I'll be using this as the profile

Next, I checked the process list

```
$ volatility -f gotham.raw --profile=Win7SP1x64 pslist
(vol2) ss@Satwik:~/vol2/vol2$ python vol.py -f ../gotham.raw --profile=Win7SP1x64 pslist
Volatility Foundation Volatility Framework 2.6.1
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------
0xfffffa80036d4040 System                    4      0    113      573 ------      0 2024-08-07 04:19:36 UTC+0000
0xfffffa8004969040 smss.exe                312      4      2       34 ------      0 2024-08-07 04:19:36 UTC+0000
0xfffffa80064e2b00 csrss.exe               400    388      9      524      0      0 2024-08-07 04:19:42 UTC+0000
0xfffffa8006433360 wininit.exe             452    388      3       81      0      0 2024-08-07 04:19:42 UTC+0000
0xfffffa80063c7b00 csrss.exe               460    444     12      535      1      0 2024-08-07 04:19:42 UTC+0000
0xfffffa8006685280 winlogon.exe            516    444      5      123      1      0 2024-08-07 04:19:43 UTC+0000
0xfffffa8006682b00 services.exe            552    452      7      228      0      0 2024-08-07 04:19:43 UTC+0000
0xfffffa80066d7060 lsass.exe               568    452      9      745      0      0 2024-08-07 04:19:44 UTC+0000
0xfffffa80066d5b00 lsm.exe                 576    452     10      154      0      0 2024-08-07 04:19:44 UTC+0000
0xfffffa8006714b00 svchost.exe             680    552     11      375      0      0 2024-08-07 04:19:45 UTC+0000
0xfffffa80048eb8e0 VBoxService.ex          744    552     13      124      0      0 2024-08-07 04:19:45 UTC+0000
0xfffffa80064de5c0 svchost.exe             804    552      8      305      0      0 2024-08-06 15:49:46 UTC+0000
0xfffffa80065c8750 svchost.exe             896    552     23      593      0      0 2024-08-06 15:49:46 UTC+0000
0xfffffa80067c4860 svchost.exe             936    552     27      610      0      0 2024-08-06 15:49:46 UTC+0000
0xfffffa80067d0b00 svchost.exe             968    552     20      651      0      0 2024-08-06 15:49:46 UTC+0000
0xfffffa80067e43e0 svchost.exe            1004    552     36     1041      0      0 2024-08-06 15:49:46 UTC+0000
0xfffffa8006816b00 svchost.exe            1064    552     18      494      0      0 2024-08-06 15:49:47 UTC+0000
0xfffffa8006905590 spoolsv.exe            1280    552     14      323      0      0 2024-08-06 15:49:48 UTC+0000
0xfffffa800691a280 svchost.exe            1308    552     16      320      0      0 2024-08-06 15:49:48 UTC+0000
0xfffffa800688cb00 svchost.exe            1400    552     10      156      0      0 2024-08-06 15:49:49 UTC+0000
0xfffffa80069a8b00 svchost.exe            1428    552     20      366      0      0 2024-08-06 15:49:50 UTC+0000
0xfffffa80038c4b00 dwm.exe                1556    936      3      110      1      0 2024-08-06 15:49:58 UTC+0000
0xfffffa8006bbeb00 explorer.exe           1172   2024     32     1017      1      0 2024-08-06 15:49:58 UTC+0000
0xfffffa80067b0b00 taskhost.exe           1328    552      8      216      1      0 2024-08-06 15:49:58 UTC+0000
0xfffffa8006c5f780 VBoxTray.exe           2240   1172     17      165      1      0 2024-08-06 15:50:00 UTC+0000
0xfffffa8006bc0b00 SearchIndexer.         2460    552     13      664      0      0 2024-08-06 15:50:06 UTC+0000
0xfffffa80038ce060 wmpnetwk.exe           2784    552     33      462      0      0 2024-08-06 15:50:32 UTC+0000
0xfffffa8006d90b00 svchost.exe            2888    552     10      372      0      0 2024-08-06 15:50:33 UTC+0000
0xfffffa8004887410 sppsvc.exe              912    552      7      157      0      0 2024-08-06 15:52:00 UTC+0000
0xfffffa800673bb00 svchost.exe            1080    552     13      385      0      0 2024-08-06 15:52:01 UTC+0000
0xfffffa8004587060 GoogleCrashHan         3044   4908      5       92      0      1 2024-08-06 16:36:44 UTC+0000
0xfffffa80039c5b00 GoogleCrashHan          408   4908      5       85      0      0 2024-08-06 16:36:44 UTC+0000
0xfffffa80044c3b00 chrome.exe             4456   4464     32     1296      1      0 2024-08-06 16:36:45 UTC+0000
0xfffffa8004463060 chrome.exe             4432   4456      8      119      1      0 2024-08-06 16:36:45 UTC+0000
0xfffffa8003daeb00 chrome.exe             4928   4456     13      234      1      0 2024-08-06 16:36:47 UTC+0000
0xfffffa8004511b00 chrome.exe             4872   4456      8      155      1      0 2024-08-06 16:36:47 UTC+0000
0xfffffa8003ff9060 chrome.exe             4612   4456     17      229      1      0 2024-08-06 16:36:55 UTC+0000
0xfffffa8004846060 taskhost.exe           3620    552      5       96      1      0 2024-08-06 16:37:06 UTC+0000
0xfffffa8004412060 chrome.exe             4204   4456     11      191      1      0 2024-08-06 16:37:16 UTC+0000
0xfffffa8004403b00 cmd.exe                3944   1172      1       20      1      0 2024-08-06 16:45:56 UTC+0000
0xfffffa8006d58060 conhost.exe            4188    460      2       53      1      0 2024-08-06 16:45:56 UTC+0000
0xfffffa8003c9c4f0 notepad.exe            2592   1172      1       58      1      0 2024-08-06 16:47:20 UTC+0000
0xfffffa8003f3cb00 chrome.exe             3764   4456     17      253      1      0 2024-08-06 17:24:44 UTC+0000
0xfffffa8003cebb00 chrome.exe             2608   4456     17      250      1      0 2024-08-06 17:24:45 UTC+0000
0xfffffa800447c060 chrome.exe             3612   4456     17      253      1      0 2024-08-06 17:24:48 UTC+0000
0xfffffa800446a9a0 chrome.exe             3172   4456     17      258      1      0 2024-08-06 17:24:52 UTC+0000
0xfffffa800443e060 chrome.exe             3704   4456     17      253      1      0 2024-08-06 17:24:55 UTC+0000
0xfffffa80047c5060 chrome.exe             4452   4456     17      270      1      0 2024-08-06 17:25:33 UTC+0000
0xfffffa8004031290 chrome.exe             4836   4456     17      241      1      0 2024-08-06 17:26:01 UTC+0000
0xfffffa8003d47860 chrome.exe             2168   4456     17      231      1      0 2024-08-06 17:27:52 UTC+0000
0xfffffa8004a7fb00 chrome.exe             3808   4456     21      254      1      0 2024-08-06 18:15:29 UTC+0000
0xfffffa800437b4d0 chrome.exe             3740   4456     12      164      1      0 2024-08-06 18:32:47 UTC+0000
0xfffffa8003e86060 taskeng.exe            4196   1004      4       89      0      0 2024-08-06 18:33:18 UTC+0000
0xfffffa80039c2490 mspaint.exe            2516   1172      7      142      1      0 2024-08-06 18:35:09 UTC+0000
0xfffffa8003d94060 svchost.exe            1648    552      7      112      0      0 2024-08-06 18:35:09 UTC+0000
0xfffffa80044d6700 SearchProtocol         4436   2460      9      285      0      0 2024-08-06 18:36:43 UTC+0000
0xfffffa80040403e0 SearchFilterHo         1496   2460      6      105      0      0 2024-08-06 18:36:43 UTC+0000
0xfffffa800491c600 audiodg.exe            4028    896      6      132      0      0 2024-08-06 18:37:14 UTC+0000
0xfffffa8003bcf420 DumpItog.exe           4960   1172      5       56      1      1 2024-08-06 18:37:17 UTC+0000
0xfffffa80045dca30 conhost.exe            4140    460      2       53      1      0 2024-08-06 18:37:17 UTC+0000
```

From the list we can see that chrome, notepad and paint were open. The command prompt was used as well

### Flag 1

Checked command history 

```bash
$ volatility -f gotham.raw --profile=Win7SP1x64 cmdscan
Volatility Foundation Volatility Framework 2.6.1
**************************************************
CommandProcess: conhost.exe Pid: 4188
CommandHistory: 0x130de0 Application: cmd.exe Flags: Allocated, Reset
CommandCount: 8 LastAdded: 7 LastDisplayed: 7
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x60
Cmd #0 @ 0x12f7c0: whoami
Cmd #1 @ 0x130110: dir
Cmd #2 @ 0x12f800: bi0s
Cmd #3 @ 0x12f820: dfirlabs
Cmd #4 @ 0x12d690: Ymkwc2N0Znt3M2xjMG0zXw==
Cmd #5 @ 0x12d6d0: azr43ln1ght.github.io
Cmd #6 @ 0x125650: Azr43lKn1ght
Cmd #7 @ 0x125680: did you find flag1?
Cmd #15 @ 0xb0158:
Cmd #16 @ 0x12ff50:
**************************************************
CommandProcess: conhost.exe Pid: 4140
CommandHistory: 0x280e10 Application: DumpItog.exe Flags: Allocated
CommandCount: 0 LastAdded: -1 LastDisplayed: -1
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x10
Cmd #15 @ 0x200158: (
Cmd #16 @ 0x27ff70: (
$ echo "Ymkwc2N0Znt3M2xjMG0zXw==" | base64 -d
bi0sctf{w3lc0m3_
```

Flag 1: `bi0sctf{w3lc0m3_`

### Flag 3 and 4

Dumped notepad memory

```bash
volatility -f gotham.raw --profile=Win7SP1x64 memdump -p 2592 -D dumps/
```

Searched for "flag" but the output was huge so I narrowed it down

```bash
(strings -a dumps/2592.dmp && strings -el dumps/2592.dmp) | grep -i flag | grep -iv flags | sort -u
```

Got `flag3 = aDBwM190aDE1Xw==` and `flag4 = YjNuM2YxNzVfeTB1Xw==`

```bash
$ echo "aDBwM190aDE1Xw==" | base6echo "aDBwM190aDE1Xw==" | base64 -d
h0p3_th15_root$ echo "YjNuM2YxNzVfeTB1Xw==" | base64 -d
b3n3f175_y0u_
```

Flag 3: `h0p3_th15_`
Flag 4: `b3n3f175_y0u_`

### Flag 5

In the search result, there was `flag5.rar` so next I found its physical offest, dumped the file and renamed it to `flag5.rar`

```bash
$ volatility -f gotham.raw --profile=Win7SP1x64 filescan | grep "flag5.rar"
Volatility Foundation Volatility Framework 2.6.1
0x000000011fdaff20     16      0 -W-r-- \Device\HarddiskVolume2\Users\bruce\Desktop\flag5.rarp\VirtualBox Dropped Files\2024-08-06T18_36_43.522668500Z\flag5.rar
$ volatility -f gotham.raw --profile=Win7SP1x64 dumpfiles -Q 0x000000011fdaff20 -D ./dumps/
```

Thrn I tried extracting for which password was needed. I got it from hashdump and cracked it using [online tool](https://crackstation.net/) after referring to [this](https://www.varonis.com/blog/penetration-testing-part-v-hash-dumping-and-cracking)

```bash
7z x flag5.rar

7-Zip 23.01 (x64) : Copyright (c) 1999-2023 Igor Pavlov : 2023-06-20
 64-bit locale=C.UTF-8 Threads:16 OPEN_MAX:1024

Scanning the drive for archives:
1 file, 4096 bytes (4 KiB)

Extracting archive: flag5.rar

WARNINGS:
There are data after the end of archive

--
Path = flag5.rar
Type = Rar5
WARNINGS:
There are data after the end of archive
Physical Size = 250
Tail Size = 3846
Solid = -
Blocks = 1
Encrypted = -
Multivolume = -
Volumes = 1
Comment = The password for the zip file is the computer's password

ERROR: Unsupported Method : flag.txt

Sub items Errors: 1

Archives with Errors: 1

Warnings: 1

Sub items Errors: 1
$ volatility -f gotham.raw --profile=Win7SP1x64 hashdump
Volatility Foundation Volatility Framework 2.6.1
Administrator:500:aad3b435b51404eeaad3b435b51404ee:10eca58175d4228ece151e287086e824:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
bruce:1001:aad3b435b51404eeaad3b435b51404ee:b7265f8cc4f00b58f413076ead262720:::
HomeGroupUser$:1002:aad3b435b51404eeaad3b435b51404ee:bda4ed0acc67d6d60540d1a20cf444c6:::
```

<img width="1305" height="514" alt="image" src="https://github.com/user-attachments/assets/c1af4a56-ec1f-4d82-8471-6862f6060973" />

From the textfile I got `bTByM18xMzMzNzQzMX0=`

```bash
$ echo "bTByM18xMzMzNzQzMX0=" | base64 -d
m0r3_13337431}
```

Flag 5: `m0r3_13337431}`

### Flag 2

Dumped paint memory

```bash
volatility -f gotham.raw --profile=Win7SP1x64 memdump -p 2516 -D ./paint_dump/
```

Tried using screenshot plugin but didn't work since there were some PIL issues. Then I tried with `foremost`

```bash
foremost -t bmp,png,jpg -i paint_dump/*.dmp -o ./images/
```

```bash
Foremost version 1.5.7 by Jesse Kornblum, Kris Kendall, and Nick Mikus
Audit File

Foremost started at Wed Dec 10 20:40:06 2025
Invocation: foremost -t all -i paint_dump/2516.dmp -o ./images/ 
Output directory: /mnt/c/Users/Meenakshi Sangeeth/Downloads/gotham/images
Configuration file: /etc/foremost.conf
------------------------------------------------------------------
File: paint_dump/2516.dmp
Start: Wed Dec 10 20:40:06 2025
Length: 470 MB (493264896 bytes)
 
Num	 Name (bs=512)	       Size	 File Offset	 Comment 

0:	00048183.gif 	      18 KB 	   24669976 	  (18759 x 14406)
1:	00189335.htm 	      257 B 	   96939899 	 
2:	00189336.htm 	      253 B 	   96940251 	 
3:	00189337.htm 	      277 B 	   96940603 	 
4:	00189337_1.htm 	      233 B 	   96940987 	 
5:	00189338.htm 	      258 B 	   96941323 	 
6:	00189339.htm 	      255 B 	   96941675 	 
7:	00189339_1.htm 	      269 B 	   96942027 	 
8:	00189340.htm 	      256 B 	   96942395 	 
9:	00189341.htm 	      233 B 	   96942747 	 
10:	00189341_1.htm 	      250 B 	   96943083 	 
11:	00189342.htm 	      247 B 	   96943435 	 
12:	00189343.htm 	      269 B 	   96943787 	 
13:	00189344.htm 	      236 B 	   96944155 	 
14:	00189344_1.htm 	      253 B 	   96944491 	 
15:	00189345.htm 	      238 B 	   96944843 	 
16:	00189346.htm 	      251 B 	   96945179 	 
17:	00189346_1.htm 	      222 B 	   96945531 	 
18:	00189347.htm 	      219 B 	   96945851 	 
19:	00189347_1.htm 	      248 B 	   96946171 	 
20:	00189348.htm 	      253 B 	   96946523 	 
21:	00189349.htm 	      249 B 	   96946875 	 
22:	00189350.htm 	      281 B 	   96947227 	 
23:	00189350_1.htm 	      241 B 	   96947611 	 
24:	00189351.htm 	      246 B 	   96947947 	 
25:	00189352.htm 	      231 B 	   96948299 	 
26:	00189352_1.htm 	      233 B 	   96948635 	 
27:	00189353.htm 	      218 B 	   96948971 	 
28:	00189354.htm 	      163 B 	   96949323 	 
29:	00184952.png 	       1 KB 	   94695920 	  (11 x 294)
30:	00184955.png 	       1 KB 	   94697168 	  (20 x 100)
31:	00352620.dll 	       1 MB 	  180541504 	 11/14/2018 11:41:16
32:	00742060.jpg 	       1 KB 	  379934720 	 
33:	00742068.jpg 	       1 KB 	  379938816 	 
34:	00742084.jpg 	       1 KB 	  379947008 	 
35:	00742120.jpg 	       1 KB 	  379965440 	 
36:	00742124.jpg 	       1 KB 	  379967488 	 
37:	00742128.jpg 	       1 KB 	  379969536 	 
38:	00742132.jpg 	       1 KB 	  379971584 	 
39:	00742140.jpg 	       1 KB 	  379975680 	 
40:	00742180.jpg 	       1 KB 	  379996160 	 
41:	00742184.jpg 	       1 KB 	  379998208 	 
42:	00742192.jpg 	       1 KB 	  380002304 	 
43:	00744502.jpg 	      919 B 	  381185024 	 
44:	00744548.jpg 	      864 B 	  381208576 	 
45:	00692962.htm 	      236 B 	  354796968 	 
46:	00693012.htm 	      231 B 	  354822432 	 
47:	00693018.htm 	      231 B 	  354825632 	 
48:	00693030.htm 	      229 B 	  354831648 	 
49:	00701733.htm 	      236 B 	  359287336 	 
50:	00701735.htm 	      236 B 	  359288360 	 
51:	00709933.png 	      413 B 	  363485720 	  (16 x 16)
52:	00709935.png 	      413 B 	  363486744 	  (16 x 16)
53:	00712824.png 	      11 KB 	  364965888 	  (1150 x 116)
54:	00712864.png 	       4 KB 	  364986368 	  (120 x 44)
55:	00722008.png 	      14 KB 	  369668096 	  (144 x 144)
56:	00724624.png 	       1 KB 	  371007488 	  (64 x 64)
57:	00742032.png 	       3 KB 	  379920384 	  (196 x 68)
58:	00742064.png 	       1 KB 	  379936768 	  (64 x 64)
59:	00742088.png 	       2 KB 	  379949056 	  (32 x 32)
60:	00742188.png 	       1 KB 	  380000256 	  (64 x 64)
61:	00744500.png 	      749 B 	  381184256 	  (64 x 64)
62:	00744610.png 	      603 B 	  381240576 	  (64 x 25)
63:	00744668.png 	      350 B 	  381270016 	  (32 x 32)
64:	00744830.png 	      958 B 	  381352960 	  (32 x 32)
65:	00744850.png 	      617 B 	  381363200 	  (8 x 7)
Finish: Wed Dec 10 20:40:17 2025

66 FILES EXTRACTED
	
jpg:= 13
gif:= 1
htm:= 34
exe:= 1
png:= 17
------------------------------------------------------------------

Foremost finished at Wed Dec 10 20:40:18 2025
```
No visible flags













