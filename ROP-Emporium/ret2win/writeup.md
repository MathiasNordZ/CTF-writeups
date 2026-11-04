# Ret2Win — ROP Emporium

Writeup for the 64-bit **ret2win** challenge from [ROP Emporium](https://ropemporium.com/challenge/ret2win.html).

1. Opened the binary in Ghidra.

    ![step1.png](images/step1.png)

2. Opened the `pwnme()` function. The function defines a 32 byte buffer called `local_28` and reads 56 bytes into it, which makes it vulnerable to a buffer overflow.

    ![step2.png](images/step2.png)

3. As we can see there is a function called `ret2win`.

    ![step3.png](images/step3.png)

4. This function calls `system("/bin/cat flag.txt")`, which prints the flag. It has the address of `0x00400756`.

    ![step4.png](images/step4.png)

5. Our binary is a 64-bit LSB with no canary, not stripped and no PIE. This makes it quite straightforward to exploit.

    ![step5a.png](images/step5a.png)

    ![step5b.png](images/step5b.png)

6. Created a script to find the offset from the buffer to the return address. This is to know how many bytes of padding is needed to return to the win address. The yielded offset was 40 bytes.

    ![step6.png](images/step6.png)

7. Made a simple exploit that inputs padding and the win address to redirect the program to our wanted function `ret2win()`.

    ![step7.png](images/step7.png)

8. Exploit works — execution was successfully redirected to `ret2win()`.

    ![step8.png](images/step8.png)
