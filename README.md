# pwn-cheatsheet
CTF pwnable challenge cheatsheet

# Cheatsheet

| Vuln Type | Protections | Limits or Specials | Method Links |
|:---------:|:-----------:|:------:|--------------|
| stack overflow | None   |  None   |  [1](#1)
| stack overflow | NX  | None | [2](#2)
| stack overflow | NX RELRO | None | [3](#3)
| stack overflow | NX RELRO CANARY | canary in forked threads | [4](#4)
| heap overflow | NX | chunk size can be 0x70~0x7f | [5](#5)
| heap overflow | NX RELRO | chunk size can't be 0x70~0x7f | [6](#6)
| heap overflow | NX RELRO | able to overwrite top size | [7](#7)

# Available Choices
## <a name="1"> 1
* [Jump sp shellcode](#jump-sp-shellcode)

## <a name="2"> 2
* [ROP mprotect-shellcode](#rop-mprotect-shellcode)
* [ROP execve](#rop-execve)

## <a name="3"> 3
* [ROP system](#rop-system)

## <a name="4"> 4
* [bruteforce canary](#bruteforce-canary)

## <a name="5"> 5
* [fastbin attack GOT](#fastbin-attack-got)

## <a name="6"> 6
* [Fastbin attack main arena bins](#fastbin-attack-main-arena-bins)
* [Tcache bypass fastbin size check](#tcache-bypass-fastbin-size-check)

## <a name="7"> 7
* [House of Force](#house-of-force-attack)

# Methods
## <a name="jump-sp-shellcode"> Use jump-sp gadget to execute shellcode on stack
Find a gadget like `jump sp` then put shellcode on stack, jump sp will jump to stack then execute shellcode.

## <a name="rop-mprotect-shellcode"> Use ROP to call mprotect then execute shellcode
Find gadgets to setup ROP chain to call mprotect on any page available, then read shellcode to that page, and then jump there and execute it.

## <a name="rop-execve"> Use ROP to call execve
Find gadgets to setup ROP chain to call read to read shell path and call execve.

## <a name="rop-system"> Use ROP to call system
Find gadgets to setup ROP chain to write GOT address(so we can get libc address) and read, then setup call to system so we are able to call shell.

## <a name="bruteforce-canary"> Bruteforce Canary
Since the canary in threads is the same as main thread so we can bruteforce the canary in sub-threads.

## <a name="fastbin-attack-got"> Fastbin Attack Got
Fastbin attack GOT can be applied when size can be range(0x70, 0x7f), since the highest byte of libc address should be 0x7f, then using cut the address trick to allocate to GOT.

## <a name="fastbin-attack-main-arena-bins"> Fastbin Attack Main Arena Bins
With ASLR on, heap address is randomized even the highest byte. We can abuse the addresses in `main_arena`'s bins(fastbins or normal bins) to allocate to `main_arena` then change meta-data in `main_arena` like top chunk address to allocate anywhere we want

## <a name="tcache-bypass-fastbin-size-check"> Use Tcache to Bypass Fastbin Size Check
This idea comes from [34c3ctf simplegc](http://blog.rh0gue.com/2018-01-05-34c3ctf-simplegc/). Since tcache will not check fastbin size when allocating and copying, we can bypass the header limits(allocating fastbin chunk's size must be consistent) check.

## <a name="house-of-force-attack"> House of Force Attack
Use house of force attack to overwrite top size to very large. Then allocate a large chunk to complete jump over the gap, so next allocation will be where we want.

# Contribution
You are welcomed to make your contribution, just use oridinary issue and pull-request methods. Try not to change the framework directly in your PR, send an issue first if you have some thoughts about that.

Currently this project is still in progress, we need you to help us. :)

Note that the `Available Choices` part we only list those that are more related to that situation. So, we don't need to repeat the methods since we have the agreement that the methods of more strict situation can be applied to easier ones.
