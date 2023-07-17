# วิธีตั้งค่า nginx config

## วิธีตั้งค่าไฟล์ใน /nginx/sites-available

เพื่อให้เว็บไซต์ของคุณทำงานได้อย่างถูกต้อง คุณจำเป็นต้องตั้งค่าไฟล์ใน /nginx/sites-available อย่างถูกต้อง ดังนั้น ในเอกสารนี้จะแนะนำวิธีการตั้งค่าไฟล์ใน /nginx/sites-available เพื่อให้เว็บไซต์ของคุณสามารถทำงานได้อย่างถูกต้อง

### ขั้นตอนการตั้งค่าไฟล์ใน /nginx/sites-available

1. เปิดไฟล์ default.conf ใน /etc/nginx/sites-available ด้วยโปรแกรมที่คุณชื่นชอบ เช่น nano, vim, หรือ emacs
2. ตั้งค่า server name โดยพิมพ์ server\_name {ชื่อโดเมนของคุณ}; ในบรรทัดที่เหมาะสม
3. ตั้งค่า root directory โดยพิมพ์ root {ที่อยู่ของไฟล์เว็บไซต์ของคุณ}; ในบรรทัดที่เหมาะสม
4. ตั้งค่า index file โดยพิมพ์ index {ชื่อไฟล์หน้าแรกของไฟล์เว็บไซต์ของคุณ}; ในบรรทัดที่เหมาะสม
5. บันทึกไฟล์และปิดโปรแกรมที่ใช้เปิดไฟล์
6. สร้าง symbolic link โดยใช้คำสั่ง ln -s /etc/nginx/sites-available/default.conf /etc/nginx/sites-enabled/
7. ทดสอบการทำงานของเว็บไซต์ของคุณด้วยการเปิดเบราว์เซอร์และพิมพ์ URL ของเว็บไซต์ของคุณ

### ตัวอย่างไฟล์ default.conf

เมื่อต้องการรองรับ nginx และ node.js ในสคริปที่อยู่ใน /nginx/sites-available คุณสามารถใช้โค้ดตัวอย่างดังนี้:

{% code overflow="wrap" %}
```bash
server {
    listen 80;
    server_name {ชื่อโดเมนหรือIP};
    root {path_project};
    index.htm index.html index.php;

	#location นี้ใช้สำหรับงานเว็บ php
	location / {
		try_files $uri $uri/ =404;
		}

        #location นี้สำหรับใช้กับโปรเจคของ node.js
	location / {
                proxy_pass <http://localhost>:{port ที่ใช้งานของ node.js};
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
                    }
}

```
{% endcode %}

ในโค้ดตัวอย่างนี้ คุณจะต้องแทนที่ {ชื่อโดเมนของคุณ} ด้วยชื่อโดเมนของคุณและ {port ที่ใช้งานของ node.js} ด้วยพอร์ตที่ใช้งานของ node.js

หลังจากนั้น คุณต้องทำการบันทึกและปิดไฟล์ และสร้าง symbolic link โดยใช้คำสั่ง ln -s /etc/nginx/sites-available/{ชื่อไฟล์ของคุณ}.conf /etc/nginx/sites-enabled/

ท้ายที่สุด คุณสามารถทดสอบการทำงานของเว็บไซต์ของคุณได้ด้วยการเปิดเบราว์เซอร์และพิมพ์ URL ของเว็บไซต์ของคุณ

### สรุป

การตั้งค่าไฟล์ใน /nginx/sites-available เป็นขั้นตอนสำคัญที่ช่วยให้เว็บไซต์ของคุณทำงานได้อย่างถูกต้อง ด้วยวิธีการตั้งค่าที่ถูกต้อง คุณสามารถให้เว็บไซต์ของคุณทำงานได้อย่างเต็มประสิทธิภาพ
