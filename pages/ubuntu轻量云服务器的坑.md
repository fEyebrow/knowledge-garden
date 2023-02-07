- 创建新用户后，如何将默认用户ubuntu的ssh迁移到新用户
	- cp /home/ubuntu/.ssh/authorized_keys /home/work/.ssh/
- 如何通过ssh远程登录
	- ```
	  vim /etc/ssh/sshd_config
	  PasswordAuthentication yes   # 去掉这句话的注释
	  #PasswordAuthentication no     # 增加这句话的注释
	  Port 22    # 打开这句话注释
	  ListenAddress 0.0.0.0     # 打开这句话注释
	  
	  service sshd reload
	  ```
-