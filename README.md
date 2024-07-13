# Seamless Migration of Peer-To-Peer QUIC Connections in Mobile Environments
An overview of the Master Thesis containing links to all implementations utilized to implement the prototype. 

**Note: Some repositories are currently in active development and might be released in future papers, therefore they are not publicly available.**

## Overview
The prototype implementation is based on 3 separate modules:

- quiche [[https://gitlab.lrz.de/von_der_beek/quiche-p2p.git](https://gitlab.lrz.de/von_der_beek/quiche-p2p.git)]
- webrtc-rs/ice [[https://github.com/JustusvonderBeek/webrtc](https://github.com/JustusvonderBeek/webrtc)]
- quicheperf [[https://gitlab.lrz.de/von_der_beek/quicheperf-stun](https://gitlab.lrz.de/von_der_beek/quicheperf-stun)]

## Build and Run
Because the **quicheperf** repository is currently not publicly available, the prototype cannot be build at the moment.

## Abstract
At the inception of the internet, its creators could not have envisioned its current scale,
importance, and requirements. Initially designed as ARPANET, a small packet switching
network designed to connect four US Universities for academic purposes, it constantly
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