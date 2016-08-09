---
layout: post
title: 'Cloudflare: ชี้โดเมนของเราไปที่ Microsoft Azure'
date: 2015-06-29 21:36:19 +0700
categories:
- internet
redirect_from: "/internet/2015/06/29/cloudflare-azure-custom-domain-name.html"
tags:
- cloud computing
- iaas
- paas
- cloudflare
- dns
---
ถ้าใครเคยสร้าง VM หรือ Web App ใน Microsoft Azure ไว้ อาจจะได้ URL เป็น http://<ชื่อที่ตั้งไว้>.cloudapp.net (กรณี VM) หรือ http://<ชื่อที่ตั้งไว้>.azurewebsites.net (กรณี Web App) ซึ่งมันยาวและไม่ค่อยเท่ห์ ถ้าหากว่ามีโดเมนเนมเป็นของตัวเอง+ใช้ Cloudflare อยู่ และอยากเปลี่ยน URL ให้จำง่ายและเท่ห์ขึ้น ก็สามารถทำได้โดยมีขั้นตอนง่ายๆดังนี้

## เตรียมข้อมูลที่จำเป็น

ข้อมูลที่ต้องใช้ก็มี 2 อย่าง ได้แก่

1.  **URL เดิมที่ติดมากับ VM หรือ Web App** ข้อมูลส่วนนี้สามารถดูได้ในหน้าจัดการของ VM หรือ Web App นั้นๆ โดยในตัวอย่างนี้จะกำหนดให้เป็น weeix.cloudapp.net
2.  **URL ใหม่** ก็คือโดเมนที่เราจดไว้นั่นเอง โดยในตัวอย่างนี้จะกำหนดให้ weeix.com และ www.weeix.com ชี้ไปที่ URL เดิมที่เตรียมไว้

[![เข้าไปดู URL เดิมได้ที่ส่วน DNS name ในหน้าจัดการ VM หรือ Web App](http://weeix.com/wp-content/uploads/2015/06/azure-vm-management-panel.png)](http://weeix.com/wp-content/uploads/2015/06/azure-vm-management-panel.png)

## เพิ่ม DNS record

เมื่อมีข้อมูลครบแล้ว ที่เหลือก็เพียงแค่เข้าไปตรงเมนู DNS ของ Cloudflare แล้วก็เพิ่ม DNS record ดังนี้

| Type  | Name | Domain name        |
|-------|------|--------------------|
| CNAME | @    | weeix.cloudapp.net |
| CNAME | www  | weeix.cloudapp.net |

**หมายเหตุ:** เครื่องหมาย @ ในระบบ DNS ของ Cloudflare หมายถึงชื่อโดเมนของเราเพียวๆโดยที่ไม่มีอะไรในหน้าเลย ในที่นี้ก็จะเป็น weeix.com (ไม่มี www)

[![เมนูสำหรับใช้จัดการ DNS record ของ Cloudflare](//weeix.com/wp-content/uploads/2015/06/cloudflare-dns.png)](//weeix.com/wp-content/uploads/2015/06/cloudflare-dns.png)

[![เพิ่ม CNAME ใหม่โดยให้ weeix.com ชี้ไปที่ weeix.cloudapp.net](//weeix.com/wp-content/uploads/2015/06/cloudflare-new-cname.png)](//weeix.com/wp-content/uploads/2015/06/cloudflare-new-cname.png)

[![DNS record ของ weeix.com และ www.weeix.com ถูกตั้งค่าให้ชี้ไปยังที่เดียวกัน คือ weeix.cloudapp.net จากรูปจะเห็นได้ว่ามีเครื่องหมายตกใจหลังคำว่า CNAME ใน record ที่ตั้ง name เป็น @ ทั้งนี้เป็นเพราะโดยปกติแล้ว ระบบ DNS มาตรฐาน จะไม่อนุญาติให้ CNAME record ชี้ไปที่โดเมนของเราเพียวๆ แต่ Cloudflare ได้ช่วยเราแก้ปัญหานี้โดยใช้เทคนิค CNAME Flattening](//weeix.com/wp-content/uploads/2015/06/cloudflare-cname-records.png)](//weeix.com/wp-content/uploads/2015/06/cloudflare-cname-records.png)

เพียงเท่านี้ เราก็สามารถเข้าเว็บผ่าน URL weeix.com หรือ www.weeix.com ได้แล้ว

จริงๆแล้วมีอีกวิธีคือนำค่า Virtual IP address มาเพิ่มเป็น A record แต่ไม่ค่อยแนะนำเท่าไหร่ เพราะเวลาที่เรา shutdown VM หรือ Web App แล้ว IP ที่ได้จะเปลี่ยนไป ทำให้ต้องเสียเวลามานั่งแก้ไข DNS กันใหม่ เว้นแต่ว่าเราจะเลือกให้ VM ของเราจอง IP ไว้ (reserved IP address) ตั้งแต่ตอนที่สร้าง VM ขึ้นมาครั้งแรก