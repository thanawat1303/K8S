# K8S

- ระบบการกระจายงานที่คล้ายกับ swarm แต่มีขนาดที่เล็กกว่า โดย appication ทำงานอยู่ภายใต้ object ที่เรียกว่า Pods
- มีการเก็บ state และ สามารถ rollbacks ย้อนกลับได้ ด้วย ReplicaSet
- กำหนดการทำงานร่วมกันผ่าน network namespace
- มี master กี่เครื่องก็ได้
- มีการ downtime น้อยที่สุด เพราะสามารถสร้าง port ใหม่เองได้

## Kube Architecture
- Control Plane 
  - API Server
    - access and manage cluster (สำหรับเข้าถึงและจัดการ cluster)

  - Controller Manager
    - control node in cluster (ควบคุม node ภายใน cluster)
  
  - Scheduler 
    - assign task to node (กระจายงานเพื่อไปทำงานบน Node)

  - Cluster DNS
    - manage object to communicate by Protocal Network (จัดการ object หรือ componant ที่ต้องสื่อสาร หรือต้องติดต่อกันผ่านทาง Protocal network ด้วย DNS)

  - ETCD
    - Insert information of cluster, such Ip address (จัดเก็บข้อมูลของ cluster เช่น Ip address มี app อะไรทำงานอยู่บ้าง)

- Worker node
  - Kubelet
    - contact by API (ติดต่อด้วย API)
    - receive command from user control pane, such resouce of container (รับคำสั่งจากผู้ใช้งานเพื่อนำคำสั่งไปจัดการสร้าง container เช่น การจอง resouce)

  - Kubeproxy
    - connect to container or pods by network(สำหรับเชื่อมต่อ pod หรือ container ผ่าน network)

## Kube kind หรือ Resources

- Pods
  - หน่วยพื้นฐานที่เล็กที่สุด เป็นการรวมกลุ่มของ container ภายใต้ namespace เดียวกัน เพื่อให้เข้าถึงข้อมูลได้ง่ายยิ่งขึ้น โดยสามารถนำ apps มาใส่ และไปทำงานบน k8s
  - kind : Pod เป็นเหมือนกำหนดว่า app จะทำงานบน pod นี้
  - ตัวอย่างไฟล์ .yaml
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod1
    spec:
      containers:
      - name: httpd
        image: httpd
    ```

- Deployments
  - ส่วนในการกำหนด scale ของ Pod ในการไปทำงานอยู่บน Node ใด และจำนวนในการสร้าง
  - ตัวอย่างไฟล์ .yaml
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: httpd
      labels:
        dc: IN
        env: dev
    spec: # กำหนด scale ที่ต้องการ
      replicas: 3
      selector:
        matchLabels:
          env: dev
      template: # ส่วนของ Pod
        metadata:
          name: pod1
          labels:
            env: dev
        spec:
          containers:
          - name: mycon1
            image: httpd
            ports:
            - containerPort: 80
    ```
- Service
  - ส่วนในการติดต่อหา pod โดยสามารถให้เข้าถึงผ่านภายนอกได้ด้วย ingress

## K8S command
- kubectl
  - ตรวจสอบ pods บน cluster
  ```
  kubectl get pods -A
  ```

  - ตรวจสอบ service
    ```
    kubectl get svc
    ```

  - ตรวจสอบ cluster
    ```
    kubectl get nodes
    ```
  
  - สร้าง namespace
    ```
    kubectl create namespace <namespace>
    ```

  - deploy ผ่านไฟล์ .yaml
    ```
    kubectl apply -f <ชื่อไฟล์ yaml>
    ```


## K8S Remote
### SSH
  - Remote to machine for create component kube
  - Requirements
    - Linux VM [setup VM](#setup-linux-vm)
      - Container runtime, such Docker or CRIO
      - 2 CPU
      - 2 GB RAM
      - conntrack
    - Windown [setup windown](#setup-windown)
    1. Check and delete or backup path .kube and .minikube on path user or root
    2. Create cluster `don't connect ssh`
       ```
       minikube start --driver=ssh --native-ssh=false --ssh-ip-address=[IP HOST] --alsologtostderr -v=4
       ```

  - Ref
    - https://minikube.sigs.k8s.io/docs/drivers/ssh/

### Setup Linux VM
  - Swap turn off
    ```
    swapoff -a
    nano /etc/fstab #comment swap
    ```
    ![](src/commentSwap.png)

    - <details>
      <summary>verify swap turn off</summary>

      ```
      free
      ```
      ![](src/swapfree.png)

      ```
      more /etc/fstab
      ```
      ![](src/more.png)

      </details>

  - Install docker
    ```
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh
    ```

  - Install socat and conntrack
    ```
    sodu apt install socat conntrack
    ```
  
  - Restart Host docker
    ```
    sudo init 6
    ```

### Setup windown
 - <details>
   <summary>Create Key on machine</summary>

   - open powershell on Administrator
   - Create ssh key
   ```
   ssh-keygen.exe -b 4096 #4096 size key
   ```

   - open file ssh and copy or export key
   ```
   more .\.ssh\id_rsa.pub
   ```

   </details>

 - <details>
   <summary>Create Key on Github</summary>

   

   </details>

### Setup ssh key on Linux
 - <details>
   <summary>Machine</summary>

   - open on Administrator
   ```
   sudo -i
   ```

   - paste or import ssh-key in file authorized_keys
   ```
   nano .ssh/authorized_keys
   ```

   </details>

 - <details>
   <summary>Github</summary>

   

   </details>

### tool on Linux
  - btop
    - check process run on linux
    - install 
      ```
      sudo snap install btop
      ```

### tool on Windown
 - Bitvise

## Ref
- https://www.youtube.com/watch?v=QJ9rM4VFK_4