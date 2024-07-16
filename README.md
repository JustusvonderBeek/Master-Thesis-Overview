# Seamless Migration of Peer-To-Peer QUIC Connections in Mobile Environments
An overview of the Master Thesis containing links to all implementations utilized to implement the prototype. 

**Note: Some repositories are currently in active development and might be released in future papers, therefore they are not publicly available.**

## Overview
The prototype implementation is based on 3 separate modules:

- quiche [[https://gitlab.lrz.de/von_der_beek/quiche-p2p.git](https://gitlab.lrz.de/von_der_beek/quiche-p2p.git)]
- webrtc-rs/ice [[https://github.com/JustusvonderBeek/webrtc](https://github.com/JustusvonderBeek/webrtc)]
- quicheperf [[https://gitlab.lrz.de/von_der_beek/quicheperf-stun](https://gitlab.lrz.de/von_der_beek/quicheperf-stun)]

Plus an additional repository containing the Mininet setup and other testing scripts.

- measurement-scripts [[https://github.com/JustusvonderBeek/master-thesis-scripts-and-measures/tree/main](https://github.com/JustusvonderBeek/master-thesis-scripts-and-measures/tree/main)]

## Build
Because the **quicheperf** repository is currently not publicly available, the prototype cannot be build at the moment.

## Run
An example run of quicheperf can look something like the following:

```bash
quicheperf client -l 127.0.0.1:10000 -c 127.0.0.1:20000 -d 100 -b 1MB --stun-urls stun.l.google.com:19302
| sec | time  | FC_av ‖ P0 Mbps       | P0 Lost/Sched/Total (sp) |
|   0 | 48:00 |  999K ‖    0.01,  12K |     0 /    4/     4 ( 0) |
| sec | time  | Total Mbps | FC_av ‖ P0 Mbps, cwnd | P0 Lost/Sched/Total (sp) ‖ P1 Mbps, cwnd | P1 Lost/Sched/Total (sp) |
|   1 | 48:01 |       1.05 |  877K ‖    0.67,  13K |     0 /  228/    92 ( 0) ‖    0.38,  10K |     0 /  127/    61 ( 0) |
|   2 | 48:02 |       1.05 |  752K ‖    0.01,  13K |     0 /    2/     2 ( 0) ‖    1.04,  10K |     0 /  342/   150 ( 0) |
|   3 | 48:03 |       1.06 |  627K ‖    0.00,  13K |     0 /    2/     2 ( 0) ‖    1.06,  10K |     0 /  352/   162 ( 0) |
|   4 | 48:04 |       1.06 |  502K ‖    0.00,  13K |     0 /    2/     2 ( 0) ‖    1.06,  10K |     0 /  341/   142 ( 0) |
| sec | time  | Total Mbps | FC_av ‖ P0 Mbps, cwnd | P0 Lost/Sched/Total (sp) ‖ P1 Mbps, cwnd | P1 Lost/Sched/Total (sp) ‖ P2 Mbps, cwnd | P2 Lost/Sched/Total (sp) |
|   5 | 48:05 |       1.07 |  377K ‖    0.00,  13K |     0 /    2/     2 ( 0) ‖    1.06,  10K |     0 /  361/   166 ( 0) ‖    0.01,  12K |     0 /    2/     2 ( 0) |
|   6 | 48:06 |       1.06 |  252K ‖    0.00,  13K |     0 /    2/     2 ( 0) ‖    1.05,  10K |     0 /  333/   137 ( 0) ‖    0.01,  12K |     0 /    2/     2 ( 0) |
|   7 | 48:07 |       1.06 |  128K ‖    0.00,  13K |     0 /    2/     2 ( 0) ‖    1.06,   5K |     0 /  367/   182 ( 0) ‖    0.00,  13K |     0 /    2/     2 ( 0) |
|   8 | 48:08 |       1.19 |   19K ‖    1.09,  22K |     0 /  337/   149 ( 0) ‖    0.06,   0  |     0 /   20/    16 ( 0) ‖    0.03,  13K |     0 /    9/     6 ( 0) |
...
|  22 | 48:22 |       1.06 |   25K ‖    0.00,   0  |     0 /    0/     0 ( 0) ‖    1.06,  45K |     0 /    0/   155 ( 0) |
|  23 | 48:23 |       1.06 |   34K ‖    0.00,   0  |     0 /    0/     0 ( 0) ‖    1.06,  46K |     0 /    0/   155 ( 0) |
|  24 | 48:24 |       1.06 |   44K ‖    0.00,   0  |     0 /    0/     0 ( 0) ‖    1.06,  47K |     0 /    0/   167 ( 0) |
|  25 | 48:25 |       1.07 |   53K ‖    0.00,   0  |     0 /    3/     3 ( 0) ‖    1.07,  47K |     0 /    0/   181 ( 0) |
|  26 | 48:26 |   28K ‖    1.07,  46K |     0 /    0/   163 ( 0) |
|  27 | 48:27 |   36K ‖    1.06,  46K |     0 /    0/   159 ( 0) |
|  28 | 48:28 |   46K ‖    1.06,  48K |     0 /    0/   170 ( 0) |
```

## Abstract
At the inception of the internet, its creators could not have envisioned its current scale,
importance, and requirements. Initially designed as ARPANET, a small packet switching
network designed to connect four US universities for academic purposes, it constantly
evolved over its 55-year history into a global real-time communication network firmly
integrated into everyday life. With the rise of smartphones, a shift towards more mobile
internet traffic is observed, requiring fast access technologies and error-tolerant transport
protocols. However, the currently widely used and reliable transport protocol TCP was
not designed with mobility in mind. In dynamic situations where devices move and
change their IP address, especially during mobile Peer-to-Peer (P2P) connections without
an intermediary server, the best TCP can do is timeout and reconnect. In addition, direct
P2P connection establishment in mobile networks is impossible without protocols like
ICE due to middleboxes such as firewalls and NATs, which block incoming network
packets by default.

To find current best practices, state, and limitations of mobile P2P applications, we
analyze four exemplary bulk data and real-time applications: AirDrop, QuickShare,
FaceTime, and WhatsApp (video calls). We examine the utilized transport protocols,
path expansion mechanisms and behavior, and robustness in changing network condi-
tions due to active path migration functionality. Based on this information, we design,
implement, and evaluate a QUIC-based general-purpose protocol solution that accounts
for NATs, firewalls, and changing mobile network conditions. The goal is to provide
a first step towards an adaptive protocol that actively searches for new network paths,
builds and maintains direct P2P backup paths in the local network and through the in-
ternet, and migrates to an alternative path in case network conditions change to provide
continuous connectivity for mobile devices with multiple network interfaces.

We evaluate the prototype implementation for NAT traversal capabilities and path
migration effectiveness in a Mininet setup and under real-world network conditions and
compare the results with the analyzed P2P applications. By this, a robust and general-
purpose solution for mobile networks is possible. In contrast to AirDrop, QuickShare,
and WhatsApp, the prototype continuously discovers, builds, and maintains new backup
network paths by integrating a continuous ICE mechanism. Therefore, path migration
takes an average of 160 ms, compared to 100 ms in FaceTime, 200 ms to 8 s in AirDrop,
and 500 ms in WhatsApp. The path expansion process takes between 533 ms and 1104 ms,
depending on the number of NATs on the path.

Additional IPv6 support, a more capable NAT traversal algorithm, and initial connec-
tion establishment in the prototype could improve upon the results achieved.