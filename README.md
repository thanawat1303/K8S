# Kube Remote

### Kube Architecture
- Control Plane 
  - API Server
    - access and manage cluster (สำหรับเข้าถึงและจัดการ cluster)

  - Controller Manager
    - control node in cluster (ควบคุม node ภายใน cluster)
  
  - Scheduler 
    - assign task to node (กระจายงาน)

  - Cluster DNS
    - manage object to communicate by Protocal Network (จัดการ object หรือ componant ที่ต้องสื่อสาร หรือต้องติดต่อกันผ่านทาง Protocal network ด้วย DNS)

  - ETCD
    - Insert information of Object, such Ip address (จัดเก็บข้อมูลของ object เช่น Ip address)

- Worker node
  - Kubelet
    - contact by API (ติดต่อด้วย API)
    - receive command from user control pane, such resouce of container (รับคำสั่งจากผู้ใช้งานเพื่อนำคำสั่งไปจัดการสร้าง container เช่น การจอง resouce)

  - Kubeproxy
    - connect to container or pods by network(สำหรับเชื่อมต่อ pod หรือ container ผ่าน network)

### SSH
  - Remote to machine for create component kube
  - Requirements
    - Linux VM
      - Container runtime, such Docker or CRIO
      - 2 CPU
      - 2 GB RAM
      - conntrack
  - Ref
    - https://minikube.sigs.k8s.io/docs/drivers/ssh/

### Ref
- https://www.youtube.com/watch?v=QJ9rM4VFK_4