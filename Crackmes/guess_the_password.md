1.  Initially checked the file information to understand what we are working with.
```
┌──(kali㉿kali)-[~/Downloads/guess_the_password]
└─$ file guess_the_password 
guess_the_password: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=4b5316bbef627d72efaa8d38723b2e9725065ec2, for GNU/Linux 3.2.0, not stripped
```

2. Ran the program to see what it does. It takes an input and likely validates if it is correct or false.
```
┌──(kali㉿kali)-[~/Downloads/guess_the_password]
└─$ ./guess_the_password 
What is the password:TEST 
Incorrect
```

3. Disassembled main and noticed the call to a function called `check` on line `+120` Will have to take a look at this function later on.
```
pwndbg> disas main
Dump of assembler code for function main:
   0x00000000000011e9 <+0>:     endbr64
   0x00000000000011ed <+4>:     push   rbp
   0x00000000000011ee <+5>:     mov    rbp,rsp
   0x00000000000011f1 <+8>:     sub    rsp,0x110
   0x00000000000011f8 <+15>:    mov    rax,QWORD PTR fs:0x28
   0x0000000000001201 <+24>:    mov    QWORD PTR [rbp-0x8],rax
   0x0000000000001205 <+28>:    xor    eax,eax
   0x0000000000001207 <+30>:    lea    rax,[rip+0xdf6]        # 0x2004
   0x000000000000120e <+37>:    mov    rdi,rax
   0x0000000000001211 <+40>:    mov    eax,0x0
   0x0000000000001216 <+45>:    call   0x10d0 <printf@plt>
   0x000000000000121b <+50>:    mov    rdx,QWORD PTR [rip+0x2dee]        # 0x4010 <stdin@GLIBC_2.2.5>
   0x0000000000001222 <+57>:    lea    rax,[rbp-0x110]
   0x0000000000001229 <+64>:    mov    esi,0x100
   0x000000000000122e <+69>:    mov    rdi,rax
   0x0000000000001231 <+72>:    call   0x10f0 <fgets@plt>
   0x0000000000001236 <+77>:    lea    rax,[rbp-0x110]
   0x000000000000123d <+84>:    lea    rdx,[rip+0xdd6]        # 0x201a
   0x0000000000001244 <+91>:    mov    rsi,rdx
   0x0000000000001247 <+94>:    mov    rdi,rax
   0x000000000000124a <+97>:    call   0x10e0 <strcspn@plt>
   0x000000000000124f <+102>:   mov    BYTE PTR [rbp+rax*1-0x110],0x0
   0x0000000000001257 <+110>:   lea    rax,[rbp-0x110]
   0x000000000000125e <+117>:   mov    rdi,rax
   0x0000000000001261 <+120>:   call   0x12a6 <check>
   0x0000000000001266 <+125>:   cmp    eax,0x1
   0x0000000000001269 <+128>:   jne    0x127c <main+147>
   0x000000000000126b <+130>:   lea    rax,[rip+0xdaa]        # 0x201c
   0x0000000000001272 <+137>:   mov    rdi,rax
   0x0000000000001275 <+140>:   call   0x10a0 <puts@plt>
   0x000000000000127a <+145>:   jmp    0x128b <main+162>
   0x000000000000127c <+147>:   lea    rax,[rip+0xda1]        # 0x2024
   0x0000000000001283 <+154>:   mov    rdi,rax
   0x0000000000001286 <+157>:   call   0x10a0 <puts@plt>
   0x000000000000128b <+162>:   mov    eax,0x0
   0x0000000000001290 <+167>:   mov    rdx,QWORD PTR [rbp-0x8]
   0x0000000000001294 <+171>:   sub    rdx,QWORD PTR fs:0x28
   0x000000000000129d <+180>:   je     0x12a4 <main+187>
   0x000000000000129f <+182>:   call   0x10c0 <__stack_chk_fail@plt>
   0x00000000000012a4 <+187>:   leave
   0x00000000000012a5 <+188>:   ret
```

4. Decomplied the binary in Ghidra and opened the main function.  As we can see there is a buffer with a size of 264 bytes. `fgets` passes the user input from standard input into the buffer. The user input is then passed into a function called `check` which is the password check function.
```C
undefined8 main(void)

{
  int iVar1;
  size_t inputLength;
  long in_FS_OFFSET;
  char userInput [264];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  printf("What is the password:");
  fgets(userInput,0x100,stdin);
  inputLength = strcspn(userInput,"\n");
  userInput[inputLength] = '\0';
  iVar1 = check(userInput);
  if (iVar1 == 1) {
    puts("Correct");
  }
  else {
    puts("Incorrect");
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
```

5. The main logic of the function is to check if the length of the input is 10 characters, if it contains an occurrence of `1` and if index `4` is equal to `9`. If all these are correct the variable `uVar1` is set equal to one. The function then returns back to main. In main, the if statement `iVar == 1` is true and it prints correct.
```C
undefined8 check(char *userInput)

{
  size_t inputLength;
  undefined8 uVar1;
  
  inputLength = strlen(userInput);
  if ((((int)inputLength == 10) && (*userInput == '1')) && (userInput[4] == '9')) {
    uVar1 = 1;
  }
  else {
    uVar1 = 0;
  }
  return uVar1;
}
```

6. Success with the input `1999999999`
```
┌──(kali㉿kali)-[~/Downloads/guess_the_password]
└─$ ./guess_the_password        
What is the password:1999999999
Correct
```

