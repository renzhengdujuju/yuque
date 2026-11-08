---
title: "部署java环境"
yuque_slug: "dggmg18uurn97nql"
yuque_id: 240276108
yuque_updated: "2026-10-14T04:52:26.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/dggmg18uurn97nql"
---
### 下载 **OpenJDK 8**

OpenJDK 是 Java 的开源实现，功能与 Oracle JDK 基本一致，推荐用于学习和生产。

**1. Adoptium（原 AdoptOpenJDK）**

- 官网：https://adoptium.net/
- 选择版本：Temurin 8 (OpenJDK 8)
- 支持平台：Windows / macOS / Linux
- 特点：由 Eclipse 基金会维护，企业级支持，完全免费。

👉 直达下载页：  
https://adoptium.net/temurin/releases/?version=8

**中间会跳出来jre的下载页面，直接删掉就好**

### 部署环境
**1.在windows里面搜索高级系统设置**

![](../_assets/39db5a4e_1760328735920-e415a6b8-554e-439e-a8f0-7a5fbca3bfff.png)

**点击环境变量**

![](../_assets/0bb67e56_1760328972114-62c12e6a-84b5-4a60-afe2-6c183b9a0fa1.png)

**在path行选择编辑**

![](../_assets/f27b7142_1760329026418-234c10bc-3198-4600-b08b-85fbc01da1e2.png)

**将下载好的jdk8的bin（jdk的执行文件目录）目录的路径添加到环境变量中**

![](../_assets/144590cd_1760328343045-d1e6851e-8f27-4b85-9106-509faad5f495.png)

**检测配置环境是否成功：**

**输入指令windows+r，输入cmd**

![](../_assets/5d7270d0_1760329231385-d055ac75-8e17-42ec-9cda-305ec7fe52e4.png)

**会跳出一个黑窗口，即windows的命令行，输入指令 java -version查看java的版本号**

![](../_assets/fe385759_1760329324495-3ad402bd-a7c1-482c-a0ce-cebf85b48715.png)

**配置环境变量的目的：**

**为了让jdk中bin路径下的可执行文件，在计算机的任意路径下都可以使用**
