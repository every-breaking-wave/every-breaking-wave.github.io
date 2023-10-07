---

title: k8s上的数据持久化
catalog: true

lang: cn
header-img: /img/header_img/lml_bg.jpg
tags:

- 经验总结

---



## NFS

### PV和PVC

- PV：

  持久卷（PersistentVolume）简称PV，是集群中的一块存储，可以由管理员事先供应。 可以配置NFS、Ceph等常用存储配置，相对于volumes，提供了更多的功能，如生命周期管理、大小的限制。 PV 卷的供应有两种方式：静态供应或动态供应。

  静态： 集群管理员预先创建许多PV，在PV的定义中能够体现存储资源的特性。

  动态： 集群管理员无须预先创建PV，而是通过StorageClass的设置对后端存储资源进行描述，标记存储的类型和特性。用户通过创建PVC对存储类型进行申请，系统将自动完成PV的创建及与PVC的绑定。如果PVC声明的Class为空""，则说明PVC不使用动态模式。

- PVC

持久卷申领（PersistentVolumeClaim，PVC）表达的是用户对存储的请求。就像Pod消耗Node的资源一样，**PVC消耗PV资源**。PVC可以申请存储空间的大小（size）和访问模式。

```YAML
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 20Gi
  accessModes:
    - ReadWriteOnce
  nfs:
    path: /root/sharedata/mysql
    server: 127.0.0.1
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi
```

### volumeMount

> 在Docker中就有数据卷的概念，当容器删除时，数据也一起会被删除，想要持久化使用数据，需要把主机上的目录挂载到Docker中去，在K8S中，数据卷是通过Pod实现持久化的，如果Pod删除，数据卷也会一起删除，k8s的数据卷是docker数据卷的扩展，K8S适配各种存储系统，包括本地存储EmptyDir,HostPath,网络存储NFS,GlusterFS,PV/PVC等，下面就详细介绍下K8S的存储如何实现。

我们可以修改mysql-server.yaml来指定持久化存储的地址。

```YAML
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort
  ports:
  - port: 3306
    nodePort: 30080 
    targetPort: 3306
  selector:
    app: mysql
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  serviceName: "mysql"
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - image: mysql:8
        name: mysql
        env:        #定义环境变量
         - name: "MYSQL_ROOT_PASSWORD"
           value: '123456'
        ports:                #容器内端口
        - containerPort: 3306
          name: mysql
        volumeMounts:                  # 容器内挂载点
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql       
      volumes:
      - name: mysql-persistent-storage        #跟上面的名称对应
        persistentVolumeClaim:
          claimName: mysql-pv-claim
```