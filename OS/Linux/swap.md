1. 创建swap文件

   ```bash
   dd if=/dev/zero of=/myswap bs=1M count=4096
   ```

2. 设置可访问权限

   ```bash
   chmod 600 /myswap
   ```

3. 格式化文件

   ```bash
   mkswap /myswap
   ```

4. 激活swap空间(每次swapon可以叠加swap空间)

   ```bash
   swapon /myswap
   ```

5. 开机自动启用swap空间，追加下面内容到/etc/fstab

   ```bash
   echo "/myswap swap swap default 0 0" >>/etc/fstab
   ```

