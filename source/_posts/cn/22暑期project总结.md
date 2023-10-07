---

title: 22暑期项目小结
catalog: true

lang: cn
header-img: /img/header_img/lml_bg.jpg
tags:

- 经验总结

---

## 前端

### Web 前端

技术栈：

- 语言： JSX+ HTML+CSS
- 框架： React
- 组件库： Antd
- 请求方式： Fetch
- 服务器：[nginx](https://blog.csdn.net/weixin_51969975/article/details/126566815?spm=1001.2014.3001.5502)

### App 前端

技术栈：

- 语言： JavaScript + HTML+CSS
- 框架： React Native
- 构建工具：Gradel
- 组件： RN elements etc.
- 存储：localStorage
- 请求方式： Fetch

## 后端

### Java后端

技术栈：

- 框架：SpringBoot
- 安全性： Spring Security、JWT
- xxxxxxxxxx void merge_sort(int q[], int l, int r){    if(l >= r) return;    int mid = (l + r) >> 1;    merge_sort(q, l, mid);    merge_sort(q, mid + 1, r);    int i = l, j = mid + 1, k = 0;    while (i <= mid && j <= r) {        if(q[i] <= q[j]) {            tmp[k++] = q[i++];        } else {            tmp[k++] = q[j++];        }    }    while (i <= mid ) tmp[k++] = q[i++];    while (j <= r) tmp[k++] = q[j++];    for (i = l, j = 0; i <= r; ++i, ++j) {        q[i] = tmp[j];    }}C
- Layers:  Controller、Service、Dao、Repository

### Python后端

技术栈：

- 环境搭建
  - Anaconda
- 深度学习模型： Bert、word2vector、HNSW
- 服务器搭建： Flask
- 部署：gunicorn

## 数据库

### MySql

- batch写入
- orm映射
- remote连接

### Redis

- 阻塞队列
- future

## 部署与运维

### [docker](https://blog.csdn.net/weixin_51969975/article/details/126310667?spm=1001.2014.3001.5502)

- maven package
- docker build(x)
- tag & push

### [k8s](https://blog.csdn.net/weixin_51969975/article/details/126308811?spm=1001.2014.3001.5502)

- 搭建节点，集群化服务
- dashboard

### nfs

[数据库持久化](https://www.wolai.com/35TCuRZ9jQPYmGM5vnhR6F)

- redis和mysql的永久化储存

### 可观测性

- [Dashboard: kubenetes 的可视化管理界面](https://blog.csdn.net/weixin_51969975/article/details/126309401?spm=1001.2014.3001.5502)

- prometheus ：监测系统指标
- [grafana：可视化prometheus](https://blog.csdn.net/weixin_51969975/article/details/126309091?spm=1001.2014.3001.5502)

-  较为方便地管理gunicorn进程，查看log



### 安全性

- Spring Security + JWT

- RBAC

### CI/CD

- 华为云提供的打包、build脚本

## 测试

### 前端

- Jest
  - mock
  - snapShoot

## 后端

- Junit
- Locust
  - cmd: locust -f [file.py](http://file.py) —host=ip:port
- restTemplate进行controller层的测试
  - postEntity
  - exchange
- Ab tools
- Swagger + Knife4j 生成接口文档