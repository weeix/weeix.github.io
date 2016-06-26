---
layout: post
title: 'Microsoft Azure: สร้าง VM Ubuntu โดยใช้หน้าจัดการแบบเก่า'
date: 2015-11-12 00:39:59 +0700
categories:
- อินเทอร์เน็ต
tags:
- cloud computing
- iaas
- microsoft
- azure
---
วันนี้จะมาสาธิตวิธีสร้าง Virtual Machine (VM) เป็น Ubuntu Server 14.04 LTS โดยใช้ Management Portal แบบเก่าครับ จริงๆ ก็ไม่แน่ใจว่าหน้าต่างมันยังเหมือนเดิมอยู่รึเปล่า เพราะเตรียม (ดอง) เนื้อหาไว้จะเกือบครึ่งปีแล้ว

เอาเป็นว่ามาเริ่มกันเลยดีกว่า ก่อนอื่น ถ้าใครยังไม่ได้สมัคร หากสนใจอยากทดลองใช้งานก็เข้าไปอ่านรายละเอียดได้ในโครงการ [Azure Free Trial](https://azure.microsoft.com/en-us/pricing/free-trial/) นะครับ คร่าวๆ คือเราเอาเลขบัตรเครดิตไปฝากไว้กับเขา เขาก็จะให้เราเข้าไปทดลองใช้บริการได้ 30 วัน โดยมีวงเงินให้ 200$

เข้าหน้าจัดการแบบเก่าผ่าน [https://manage.windowsazure.com](https://manage.windowsazure.com) ก็จะเห็นหน้าต่างประมาณนี้

[![หน้าจัดการแบบเก่า](https://weeix.com/wp-content/uploads/2015/06/017-classic-home-1024x600.png)](//weeix.com/wp-content/uploads/2015/06/017-classic-home.png)

เราจะเริ่มสร้าง VM กัน โดยกดปุ่ม NEW ที่มุมล่างซ้าย แล้วเลือก COMPUTE > VIRTUAL MACHINE > FROM GALLERY

[![เมนู NEW](https://weeix.com/wp-content/uploads/2015/11/018-classic-new-vm-1024x298.png)](https://weeix.com/wp-content/uploads/2015/11/018-classic-new-vm.png)

ขั้นแรก เขาก็จะถามว่าเราต้องการใช้บริการแบบไหน ระบบปฏิบัติการอะไร ในที่นี้เราจะเลือกเป็น Ubuntu 14.04 LTS

[![เลือก Image](https://weeix.com/wp-content/uploads/2015/11/019-classic-new-vm01.png)](https://weeix.com/wp-content/uploads/2015/11/019-classic-new-vm01.png)

ต่อมาจะเป็นการตั้งชื่อ VM เลือกสเป็ค (CPU และ RAM) แล้วก็ชื่อผู้ใช้ครับ จะเห็นได้ว่าผมเลือก UPLOAD COMPATIBLE SSH KEY ... พร้อมทั้งอัพโหลด public key ของผมขึ้นไปด้วย เพื่อที่จะได้ [ssh เข้า vm โดยที่ไม่ต้องใส่รหัสผ่าน](http://rtsp.us/ssh-public-key-authentication/) ครับ

[![ตั้งค่า VM](https://weeix.com/wp-content/uploads/2015/11/020-classic-new-vm02.png)](https://weeix.com/wp-content/uploads/2015/11/020-classic-new-vm02.png)

จากนั้นก็ตั้ง DNS name ที่เราจะใช้เข้าถึง VM ของเราผ่านอินเตอร์เน็ตครับ Microsoft Azure จะแปลกกว่าผู้ให้บริการ IaaS เจ้าอื่นนิดนึงตรงที่เขาจะไม่แจกไอพีให้เราตรงๆ แต่จะให้ชื่อ DNS name อย่าง weeix.cloudapp.net มาแทน ทำให้เวลาเราไปแก้ไข DNS record เพื่อชี้โดเมนมาที่ VM เนี่ย จะแตกต่างออกไปนิดนึง คือแทนที่จะใช้ A record ชี้ไปที่ไอพี ก็เปลี่ยนเป็นใช้ CNAME record ชี้ไปที่ DNS name แทน สำหรับรายละเอียดสามารถไปหาอ่านได้ในบทความ [Cloudflare: ชี้โดเมนของเราไปที่ Microsoft Azure](http://weeix.com/cloudflare-azure-custom-domain-name/)

[![ตั้งค่า VM](https://weeix.com/wp-content/uploads/2015/11/021-classic-new-vm03.png)](https://weeix.com/wp-content/uploads/2015/11/021-classic-new-vm03.png)

ที่เหลือก็ไม่มีอะไรมาก กดเครื่องหมายถูกที่มุมล่างขวาของหน้าต่างเพื่อเริ่มสร้าง VM

[![สร้าง VM](https://weeix.com/wp-content/uploads/2015/11/022-classic-new-vm04.png)](https://weeix.com/wp-content/uploads/2015/11/022-classic-new-vm04.png)

ระบบก็จะเริ่มสร้าง VM ให้รอจนกว่าจะเสร็จ

[![ระบบกำลังสร้าง VM](https://weeix.com/wp-content/uploads/2015/11/023-classic-new-vm05.png)](https://weeix.com/wp-content/uploads/2015/11/023-classic-new-vm05.png)

พอ VM ที่เราเพิ่งสร้างมี STATUS เป็น Running แสดงว่าระบบได้สร้างให้เราเสร็จแล้ว ทีนี้ก็สามารถ ssh เข้าไปใช้งานได้ตามปกติเลยครับ

[![024-classic-new-vm06](https://weeix.com/wp-content/uploads/2015/11/024-classic-new-vm06.png)](https://weeix.com/wp-content/uploads/2015/11/024-classic-new-vm06.png)

ถ้าใครยังเข้าไม่ได้ ให้กดเข้าไปในรายการ VM แล้วเลือกแท็ป ENDPOINT เพื่อดูว่ามีรายการของพอร์ต TCP 22 อยู่รึเปล่า ถ้าไม่มีก็ให้สร้างขึ้นมาเสีย โดย ENDPOINT นี้จะทำหน้าที่เหมือน Firewall นั่นแหละครับ หาก VM ของเราให้บริการเว็บด้วยก็สามารถมาเปิดพอร์ต 80 กับ 443 เพิ่มเติมได้ในหน้าจอนี้

[![Forward port 22](https://weeix.com/wp-content/uploads/2015/11/025-classic-endpoints.png)](https://weeix.com/wp-content/uploads/2015/11/025-classic-endpoints.png)

ราตรีสวัสดิ์ครับ
