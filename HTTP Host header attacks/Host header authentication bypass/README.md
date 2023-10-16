## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/fa662ef2-4898-4b83-baba-5b5fb8b65002)

## Solution :

When we try to visit the admin panel at **/admin**, we get the following message.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1abe3918-0175-4e8a-a53c-089f74060baa)

The request sent is as follows,

```http
GET /admin HTTP/2
Host: 0ab100ba037872b9829b9c550011006b.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36 
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: session=vBGYtdrjVCV4YoweJ5gxZkFNscn68Wkz; _lab=46%7cMCwCFAG5RH3bQFt%2b8YBc%2fq2vLwdCh%2bq%2fAhQE5KcrcFLUn6rgL0NxHd2jiLwzIRcvbIBlqU0RsdnO00fplnQORNpAxFwunn0Bi5NGOXOY13I5UfX2mvoZHks%2fjpndLMrSHpuCQJpGPrfaif1n9YCdfxNBonofNGCpFLGFUFmZNmed4hY%3d
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Cache-Control: no-transform
X-Real-Ip: spoofed.bo3r56oiy7e8ydl82hojb4q2zt5k3ir7.oastify.com
Referer: http://qtn6altx3mjn3sqn7wtygjvh48az8ywn.oastify.com/ref
X-Client-Ip: spoofed.vxwbeqx27rns7xusb1x3kozm8de4c40t.oastify.com
X-Forwarded-For: spoofed.5iolz0ics182s7f2wbid5ykwtnzexfl4.oastify.com
Forwarded: for=spoofed.pd65ukdwnl3mnramrvdx0ifgo7uys0gp.oastify.com;by=spoofed.pd65ukdwnl3mnramrvdx0ifgo7uys0gp.oastify.com;host=spoofed.pd65ukdwnl3mnramrvdx0ifgo7uys0gp.oastify.com
X-Originating-Ip: spoofed.k5q0mf5rfgvhfm2hjq5ssd7bg2mtkw8l.oastify.com
True-Client-Ip: spoofed.npg36ipuzjfkzpmk3tpvcgre056w40sp.oastify.com
Client-Ip: spoofed.jxkzeexq7fng7lugbpxrkcza81escx0m.oastify.com
Contact: root@waacrra3ks0tky7to2a4xpcnler5qvek.oastify.com
From: root@vpob6qp2zrfszxms31p3corm0d645vtk.oastify.com
X-Wap-Profile: http://czfsg7zj98p99ew9dizkm513auglfd32.oastify.com/wap.xml
Cf-Connecting_ip: spoofed.osk49jsv2kil2qpl6uswfhuf369x8qwf.oastify.com
```

Seems that only local users can access the admin panel.

So we can change the header **Host: 0ab100ba037872b9829b9c550011006b.web-security-academy.net** to **Host: localhost** & forward the request.

Now we can access the admin panel. 

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/30ba611e-7ea8-4ef4-8934-ab35f6e372fd)

Delete the user carlos to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/778d4aa2-31a6-47b6-9593-7c89454754b2)
