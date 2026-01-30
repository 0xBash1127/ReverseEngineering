# CryMore

| Name                                                             | Author | Language | Arch | Difficulty |
| ---------------------------------------------------------------- | ------ | -------- | ---- | ---------- |
| [CryMore](https://crackmes.one/crackme/69778aa352ed08086d855c77) | b0bd0g | C/C++    | x86  | 1.5        |
### Tools Used
- [Ghidra](http://github.com/NationalSecurityAgency/ghidra)

### Writeup 
- Running `./crymore`, we get the output "Malware executed. You are screwed. Have a nice day." leading me to believe that an input is not expected to be passed as an argument when executing the binary. 
  - <img width="422" height="63" alt="image" src="https://github.com/user-attachments/assets/d5058b70-7339-4748-b2c0-f5eab3289b8c" />

- Running the strings on the binary with the option `-n 5` reveals that this binary is attempting an HTTP request for the `/neutralize` page on localhost. We can confirm this by dropping the binary into Ghidra and examining it.
  -  <img width="458" height="737" alt="image" src="https://github.com/user-attachments/assets/8a54b6c8-82f6-4237-ae1d-680d63602ed6" />
- Examining Ghirda, we can see the binary does send a GET request to localhost on port 44333 for the `/neutralize` page. Using this information, we can set up a Python HTTP server and host the neutralize page.
  - <img width="728" height="200" alt="image" src="https://github.com/user-attachments/assets/cfe7fefe-7945-4b0b-85e0-62a120a7e362" />
### Steps to reproduce
1. Create an empty file named `neutralize`
```bash
touch neutralize
```
  - <img width="389" height="151" alt="image" src="https://github.com/user-attachments/assets/531f060e-e295-4cff-a673-5654f05dbdd3" />

2. Start a Python HTTP server in your current working directory on port 44333
```
python3 -m http.server 44333
```
  - <img width="532" height="82" alt="image" src="https://github.com/user-attachments/assets/740e0c0a-f51a-442d-85a6-9e7a4c12e79d" />

3. Run the binary
```
./crymore
```
- Once the binary executes, we receive a GET request to the HTTP server for the `/neutralize` page. Since the GET request finds the `/neutralize`, we effectively solve the crackme. 
  -  <img width="682" height="476" alt="image" src="https://github.com/user-attachments/assets/094a4c8a-6c53-4658-b686-87758da2f3fd" />
