[toc]

# RHEL9系统安装

> @File: RHEL9系统安装.md
>
> @Editor: PyCharm
>
> @Author: [Austin (From Chengdu.China)](https://fairy.host)
>
> @HomePage: [AustinFairyland](https://github.com/AustinFairyland)
>
> @OperatingSystem: Windows 11 Professional Workstation 23H2 Canary Channel
>
> @CreatedTime: 2024-02-22

[![Author](https://img.shields.io/badge/Author-Austin-orange)](https://t.me/FairyLtd) [![github](https://img.shields.io/badge/Github-Austin.D-green)](https://github.com/AustinFairyland) [![GitBook](https://img.shields.io/badge/GitBook-Austin.D-green)](https://interestingbooks.gitbook.io/) [![Editor](https://img.shields.io/badge/Editor-PyCharm-yellow)](https://github.com/AustinFairyland) [![Language](https://img.shields.io/badge/Language-Markdown-orange)](https://github.com/AustinFairyland) [![Version](https://img.shields.io/badge/Version-Release-blue)](https://github.com/AustinFairyland) [![Docs](https://img.shields.io/badge/Docs-Passing-brightgreen)](https://github.com/AustinFairyland) [![Type](https://img.shields.io/badge/Type-Documents-blue)](https://github.com/AustinFairyland) [![Wakatime](https://wakatime.com/badge/user/fa851759-c657-4b1e-8bcb-3ec3a693a2cd.svg)](https://wakatime.com/@fa851759-c657-4b1e-8bcb-3ec3a693a2cd) [![Sign](https://img.shields.io/badge/%E7%AD%89%E6%88%91%E4%BB%A3%E7%A0%81%E7%BC%96%E6%88%90-%E5%A8%B6%E4%BD%A0%E4%B8%BA%E5%A6%BB%E5%8F%AF%E5%A5%BD-red)](https://github.com/AustinFairyland)

---

# 1. 环境说明

- 操作系统镜像: rhel-9.3-x86_64-dvd.iso [下载](https://developers.redhat.com/products/rhel/overview) 需登录 RedHat

# 2. 安装系统

## 2.1. 引导 ISO 进行加载

![image](https://picsur.cloud.fairies.ltd/i/74435304-1c82-4e44-8a3b-cced72769625.jpg)

## 2.2. 选择安装的磁盘进行自定义分区

1 选择磁盘

![image](https://picsur.cloud.fairies.ltd/i/bc558659-95be-4b5d-8aea-53d8337953eb.jpg)

2 选择自定义分区

![image](https://picsur.cloud.fairies.ltd/i/070337ab-2e77-4d8b-8ace-b6b3bafb3ee8.jpg)

## 2.3. 进行手动分区

**至少进行2个分区: 即 `/boot` 和 `/`  
注意: `/boot` 分区不可以设置为 LVM 逻辑卷组**

参考分区

第一个挂载分区建议为: `/boot` 分区, 容量建议: `1G` - `4G`

![image](https://picsur.cloud.fairies.ltd/i/5bf3178e-4095-4cf4-b54b-f840abea582b.jpg)

挂载 `swap` 分区, 容量建议为自己服务器内存的 `1.5倍` - `2倍`

![image](https://picsur.cloud.fairies.ltd/i/6a8aa021-a3c1-4623-b548-0025cd0da872.jpg)

挂载 `/` 分区, 容量建议: 其他剩余容量全部挂载到 `/`  
如果挂载了 `/home` 分区, 可以根据自己需求来进行合理容量分配

![image](https://picsur.cloud.fairies.ltd/i/c3c79278-20b1-4342-b959-1feeebbb7eca.jpg)

格式化磁盘保存并退出

![image](https://picsur.cloud.fairies.ltd/i/bbed7e0c-d7ed-47ab-be68-e834aa9d09ad.jpg)

## 2.4. 选择安装的软件

![image](https://picsur.cloud.fairies.ltd/i/c2a30951-3909-4eff-9fce-72f014cb783a.jpg)

类似在安装 Windows 过程中选择要安装家庭版, 教育版, 专业版, 企业版, 专业工作站版...  
我们一般选择 `服务器`

![image](https://picsur.cloud.fairies.ltd/i/525ffb25-465c-4cc7-bd74-bde6dfb83a8f.jpg)

## 2.5. 配置时区和语言

![image](https://picsur.cloud.fairies.ltd/i/65eac417-3882-4a9d-94e3-1d4a56c234ac.jpg)

配置语言

![image](https://picsur.cloud.fairies.ltd/i/1e713b4a-2c90-417e-ae92-e7f07f22c698.jpg)

配置时区和时间日期

![image](https://picsur.cloud.fairies.ltd/i/cc35a659-946f-4256-9c35-a69c44ec9737.jpg)

## 2.6. 配置 root 密码和其他用户

添加 root 密码

![image](https://picsur.cloud.fairies.ltd/i/e85f5cc6-a567-4299-974c-9ed74a974ede.jpg)

添加普通用户

![image](https://picsur.cloud.fairies.ltd/i/3a17f05e-c084-450c-b76c-9f58c6332fb2.jpg)

可选  
把普通用户添加到 root 用户组

![image](https://picsur.cloud.fairies.ltd/i/d98cf564-8d26-443d-8f14-ee6d9dba1dd7.jpg)

## 2.7 开始安装

![image](https://picsur.cloud.fairies.ltd/i/9544bb7f-d7fe-4f08-ae5d-adff8724bd25.jpg)

等待安装完成后重启

![image](https://picsur.cloud.fairies.ltd/i/07c465f3-41dc-4b8b-bf03-b19fe9b19ea1.jpg)

# 3. 完成安装

![image](https://picsur.cloud.fairies.ltd/i/8d19776d-ea42-4146-a638-cfcd8930caf9.jpg)

