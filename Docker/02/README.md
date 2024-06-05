# Docker Swarm Project

## Description
   ```txt
   پروژه داکر:

   یک کلاستر سوارم با تعداد ۲ورکر پیاده سازی کنید و سپس با استفاده از ایمیج tomcat:8.0 یک stack ایجاد نمایید و با تنظیمات زیر آن را مقداردهی نمایید.

   ۱- ایجاد سرویس با نام tomcat-stack، با در نظر گرفتن پورت 8080 و با قید اجرا بر روی worker ها
   ۲- ابتدا به صورت همزمان ۲ نسخه جدید را آپدیت نماید و سپس نسخه ی قبلی را متوقف کند.
   ۳- میزان منابع هر سرویس  cpu=0.5 و memory=512M  باشد.
   ۴- هم‌چنین در شرایط failure با تاخیر 5 ثانیه ریستارت انجام شود.
   ۵- حداکثر تعداد replica برای هر نود ۳ باشد.

   لطفا فایل stack.yml ساخته شده را ارسال نمایید و خروجی کامندهای استفاده شده را در داکیومنت مربوطه پیوست کنید.
   ددلاین پروژه: ۱۰ خرداد
   ```

## Cluster INIT
we need 2 linux machines, 1 for control and 1 for node1 with docker installed.

1. we execute this command on the comtrol machine:

    ```bash
    docker swarm init 
    ```

    it gives a join command for the worker node to join the swarm cluster:

    ```bash
    docker swarm join --token mytoken control:2377
    ```

2. check if the node(s) have successfully joined the cluster.

   ```txt
   docker node ls
   ```

## Deploy the stack
3. deploy the tomcat stack on the cluster.

   ```bash
   docker stack deploy -c stack.yml tomcat-stack
   ```

4. to check the status of services and containers of tomcat-stack in the cluster :

   ```bash
   docker stack services tomcat-stack
   ```

   ```bash
   docker stack ps tomcat-stack
   ```

