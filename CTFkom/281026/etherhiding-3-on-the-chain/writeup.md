**Task Description:**

**Solution Steps:**
1. Opened the challenge pcap in Wireshark.![[Pasted image 20261029001511.png]]
2. Opened Statistics -> HTTP -> Requests to see what HTTP requests has been sent. This is because Etherhiding usually operates over HTTP/HTTPS (Application layer).![[Pasted image 20261029001742.png]]
3. I noticed that the row containing `bnbchain.org` stands out as the most likely pivot point.![[Pasted image 20261029002212.png]]
4. The full returned URL was `http://data-seed-prebsc-1-s1.bnbchain.org/rpc`.![[Pasted image 20261029002611.png]]
5. FLAG: `CTFkom{http://data-seed-prebsc-1-s1.bnbchain.org/rpc}`