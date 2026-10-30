1. Opened the binary in Ghidra![[Pasted image 20261030225214.png]]
2. Opened the `pwnme()` function. The function defines a 32 byte buffer called `local_28`. It reads 56 bytes into a 32 byte buffer, which can lead to a possible buffer overflow exploit. 
![[Pasted image 20261030225334.png]]
3. As we can see there is a function called `ret2win`.
![[Pasted image 20261030225701.png]]
4. This function returns a file called `flag.txt`, which is the most likely success path. It has the address of `0x00400756`
![[Pasted image 20261030225757.png]]
5. Our binary is a 64-bit LSB with no canary, not stripped and no PIE. This makes it quite straight forward to exploit.
![[Pasted image 20261030225950.png]]
![[Pasted image 20261030230017.png]]
6. Created a function to find the offset from the buffer to the return address. This is to know how many bytes of padding is needed to return to the win address. The yielded offset was 40 bytes.
![[Pasted image 20261030230147.png]]
7. Made a simple exploit that inputs padding and win address to redirect the program to our wanted function `pwnme()`.
![[Pasted image 20261030230405.png]]
8. Sucessfully got the "flag".
![[Pasted image 20261030230430.png]]