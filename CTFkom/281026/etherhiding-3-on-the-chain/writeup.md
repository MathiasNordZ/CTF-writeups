---
layout: writeup
title: "Etherhiding 3: On the Chain"
category: CTFkom 2026
tags: [network]
date: 2026-10-29
permalink: /CTFkom/281026/etherhiding-3-on-the-chain/
description: Following the Etherhiding chain from HTTP statistics to the pivot URL.
---

**Task Description:**

**Solution Steps:**
1. Opened the challenge pcap in Wireshark. ![](step-1.png)
2. Opened Statistics -> HTTP -> Requests to see what HTTP requests has been sent. This is because Etherhiding usually operates over HTTP/HTTPS (Application layer).![step-2](step-2.png)
3. I noticed that the row containing `bnbchain.org` stands out as the most likely pivot point.![step-3](step-3.png)
4. The full returned URL was `http://data-seed-prebsc-1-s1.bnbchain.org/rpc`.![step-4](step-4.png)
5. FLAG: `CTFkom{http://data-seed-prebsc-1-s1.bnbchain.org/rpc}`