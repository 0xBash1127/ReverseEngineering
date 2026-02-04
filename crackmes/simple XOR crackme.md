# simple XOR crackme

| Name                                                             | Author | Language | Arch | Difficulty |
| ---------------------------------------------------------------- | ------ | -------- | ---- | ---------- |
| [simple XOR crackme](https://crackmes.one/crackme/6946be18523504d8c842eb7e)) | YandereMia | C/C++    | x86  | 2.2        |
### Writeup
- Running the binary `./crackme` reveals the following output:

  -  <img width="462" height="71" alt="image" src="https://github.com/user-attachments/assets/aaa1e48c-044e-4bc6-a3f3-ffcf6c38f219" />

- This binary compares user input to an XOR operation against a key of `0x20`. If the input does not match, the binary will print `Nope` and exit. Looking at the binary, we can see that several values are stored in arrays before the XOR operation.

  - <img width="396" height="715" alt="image" src="https://github.com/user-attachments/assets/54b52a85-22bd-4922-a5c9-685ba86daae8" />
  
 - To save some time We can examine these values in `pwndbg`.

    - <img width="931" height="487" alt="image" src="https://github.com/user-attachments/assets/538ebf81-4efa-4c10-88f6-44c0693519ce" />
    
    - XREFing with Ghidra, we can assume the following variables map to these registers: `RDX`: pbvar2, `RDI`:pbvar3, `RSI`:pbvar5, and `R8`:pbbvar4. Below that the `DISASM` window confirms the XOR operation.

### Solution

- To solve this challenge, we need to write a script to perform the XOR operation and output the result, which we will then pass as an argument to the binary.
```python
key = 0x20
pbvar2= [94, 54, 50, 40, 65, 121, 38, 51, 96, 114, 55, 106, 124, 81, 125, 62]
pbvar3= [51, 75, 112, 42, 51, 43, 78, 100, 106, 120, 95, 41, 64, 107, 100, 78]
pbvar5= [54, 105, 117, 55, 40, 105, 85, 66, 112, 68, 36, 57, 75, 108, 73, 67]
pbvar4= [58, 118, 84, 51, 63, 91, 90, 125, 99, 86, 39, 111, 102, 56, 63, 67]

for i in range(16):
    solution = pbvar2[i] ^ pbvar3[i] ^ pbvar5[i] ^ pbvar4[i] ^ key
    print(chr(solution),end="") 

```
