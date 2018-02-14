# pwn-cheatsheet
CTF pwnable challenge cheatsheet

# Cheatsheet

| Vuln Type | Protections | Limits or Specials | Method Links |
|:---------:|:-----------:|:------:|--------------|
| stack overflow | None   |  None   |  [1](#1)
| stack overflow | NX  | None | [2](#2)
| stack overflow | NX RELRO | None | [3](#3)
| stack overflow | NX RELRO CANARY | canary in forked threads | [4](#4)

# Available Choices
## <a name="1"> 1
* [Jump-sp-shellcode](#jump-sp-shellcode)
* [ROP-mprotect-shellcode](#rop-mprotect-shellcode)
* [ROP-execve](#rop-execve)
* [ROP-system](#rop-system)

## <a name="2"> 2
* [ROP-mprotect-shellcode](#rop-mprotect-shellcode)
* [ROP-execve](#rop-execve)
* [ROP-system](#rop-system)

## <a name="3"> 3
* [ROP-mprotect-shellcode](#rop-mprotect-shellcode)
* [ROP-execve](#rop-execve)
* [ROP-system](#rop-system)

## <a name="4"> 4
* [bruteforce-canary](#bruteforce-canary)

# Methods
## <a name="jump-sp-shellcode"> Jump-SP-shellcode
Find a gadget like `jump sp` then put shellcode on stack, jump sp will jump to stack then execute shellcode.

## <a name="rop-mprotect-shellcode"> ROP-mprotect-shellcode
Find gadgets to setup ROP chain to call mprotect on any page available, then read shellcode to that page, and then jump there and execute it.

## <a name="rop-execve"> ROP-execve
Find gadgets to setup ROP chain to call read to read shell path and call execve.

## <a name="rop-system"> ROP-system
Find gadgets to setup ROP chain to write GOT address(so we can get libc address) and read, then setup call to system so we are able to call shell.

## <a name="bruteforce-canary"> Bruteforce-Canary
Since the canary in threads is the same as main thread so we can bruteforce the canary in sub-threads.

# Contribution
You are welcomed to make your contribution, just use oridinary issue and pull-request methods. Try not to change the framework directly in your PR, send an issue first if you have some thoughts about that.

Currently this project is still in progress, we need you to help us. :)
