# bankconsortiumbcnet-multihost( kubernetes cluster, kafka, zookeeper, multi orderer)

[practical implementation video link](https://youtu.be/ngGU33v4E9Y)

**In This (BANKCONSORTIUMBCNET) Blockchain Network UseCase :-** 

1. Initially there were **3** orgs (banks) **sbi**,**hdfc**,**icici** having **1**, **2**, and **4** peers respectively. There were **3** channels first **chsbihdfc** (in which initially sbi and hdfc are present), second **chhdfcicici** (in which initially hdfc and icici are present) and third **chicicisbi** (in which initially icici and sbi are present).

2. Then I had **UP** the initial blockchain network which consists of **3 ORGs, 5 PEERS, 3 CAs, 3 CHANNELS**.

3. After this I have joined the peers.

4. Added new peer to exsisting org.

5. Created the new org **pnb** (bank) to exsisting blockchain network with 2 peers.

6. Created new channel.

7. Adding org to channel in which it is not present.

8. Then install and instantiate chaincodes on peers.


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

                                @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
                                @@@@@ UseCase Example implementation  @@@@@
                                @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

-----------------------------------------------------------------------------------------------------------------------------
                                               Kubernetes Cluster
-----------------------------------------------------------------------------------------------------------------------------

### Commands on Master Node

**ubuntu@kube-master:~$ sudo su**

**ubuntu@kube-master:~# kubeadm init --apiserver-advertise-address=172.16.10.161 --pod-network-cidr=10.0.0.0/24**

**ubuntu@kube-master:~# exit**

**ubuntu@kube-master:~$ mkdir -p $HOME/.kube**

**ubuntu@kube-master:~$ sudo cp -ivf /etc/kubernetes/admin.conf $HOME/.kube/config**

**ubuntu@kube-master:~$ sudo chown $(id -u):$(id -g) $HOME/.kube/config**

**ubuntu@kube-master:~$ export kubever=$(kubectl version | base64 | tr -d '\n')**

**ubuntu@kube-master:~$ kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$kubever"**

**ubuntu@kube-master:~$ kubectl get pods -o wide --all-namespaces**

-----------------------------------------------------------------------------------------------------------------------------

### Commands on Slave Nodes

**ubuntu@kube-slave1:~$ sudo su**

**root@kube-slave1:/home/ubuntu# kubeadm join 172.16.10.161:6443 --token p483ys.j4pik520e1e6g07z --discovery-token-ca-cert-hash sha256:ad01deba0311f6ff9d1545a3a638bf579dcb192b9cca1386917c98f773533551**

-----------------------------------------------------------------------------------------------------------------------------

**ubuntu@kube-slave2:~$ sudo su**

**root@kube-slave2:/home/ubuntu# kubeadm join 172.16.10.161:6443 --token p483ys.j4pik520e1e6g07z --discovery-token-ca-cert-hash sha256:ad01deba0311f6ff9d1545a3a638bf579dcb192b9cca1386917c98f773533551**

-----------------------------------------------------------------------------------------------------------------------------
                                                Command to see the pods of blockchain network
-----------------------------------------------------------------------------------------------------------------------------
**ubuntu@kube-master:~$ kubectl get pods -o wide --all-namespaces**

NAMESPACE             NAME                                                   READY   STATUS      RESTARTS   AGE   IP              NODE          NOMINATED NODE

bankconsortiumbcnet   ca-hdfc-bankconsortiumbcnet-com-578b6597bb-zdj5g       1/1     Running     0          50m   10.40.0.5       kube-slave2   <none>

bankconsortiumbcnet   ca-icici-bankconsortiumbcnet-com-fc8c4bb8-xjlbk        1/1     Running     0          50m   10.36.0.5       kube-slave1   <none>

bankconsortiumbcnet   ca-pnb-bankconsortiumbcnet-com-6cb7c66fd8-tbb6r        1/1     Running     0          34m   10.40.0.10      kube-slave2   <none>

bankconsortiumbcnet   ca-sbi-bankconsortiumbcnet-com-649948f48d-vbl9j        1/1     Running     0          50m   10.40.0.4       kube-slave2   <none>

bankconsortiumbcnet   createchannel-chhdfcicici-8lrth                        0/2     Completed   0          41m   10.36.0.9       kube-slave1   <none>

bankconsortiumbcnet   createchannel-chicicisbi-fsz8w                         0/2     Completed   0          41m   10.36.0.10      kube-slave1   <none>

bankconsortiumbcnet   createchannel-chsbihdfc-kfhgv                          0/2     Completed   0          41m   10.40.0.8       kube-slave2   <none>

bankconsortiumbcnet   ftnw-artifacts-generate-sstp5                          0/2     Completed   0          50m   10.40.0.0       kube-slave2   <none>

bankconsortiumbcnet   ftnw-copyartifacts-wswl2                               0/1     Completed   0          51m   10.36.0.0       kube-slave1   <none>

bankconsortiumbcnet   gc-pnb-9q2rf                                           0/1     Completed   0          34m   10.36.0.10      kube-slave1   <none>

bankconsortiumbcnet   gc-sbi-peer1-6lflx                                     0/1     Completed   0          38m   10.40.0.10      kube-slave2   <none>

bankconsortiumbcnet   jaotc-pnb-chall-part1-5fhf5                            0/1     Completed   0          32m   10.36.0.11      kube-slave1   <none>

bankconsortiumbcnet   jaotc-pnb-chall-part2-tblnm                            0/1     Completed   0          32m   10.40.0.12      kube-slave2   <none>

bankconsortiumbcnet   jaotc-sbi-chall-part1-k2pfp                            0/1     Completed   0          30m   10.40.0.12      kube-slave2   <none>

bankconsortiumbcnet   jaotc-sbi-chall-part2-xq8zg                            0/1     Completed   0          30m   10.36.0.11      kube-slave1   <none>

bankconsortiumbcnet   jc-hdfc-peer0-chsbihdfc-ch4fh                          0/1     Completed   0          39m   10.40.0.8       kube-slave2   <none>

bankconsortiumbcnet   jc-pnb-peer0-chall-4gmg2                               0/1     Completed   0          31m   10.36.0.11      kube-slave1   <none>

bankconsortiumbcnet   jc-sbi-peer0-chall-tpsgj                               0/1     Completed   0          29m   10.40.0.12      kube-slave2   <none>

bankconsortiumbcnet   jc-sbi-peer0-chicicisbi-npjdb                          0/1     Completed   0          38m   10.40.0.8       kube-slave2   <none>

bankconsortiumbcnet   jc-sbi-peer0-chsbihdfc-nd264                           0/1     Completed   0          40m   10.40.0.8       kube-slave2   <none>

bankconsortiumbcnet   jc-sbi-peer1-chsbihdfc-rkz5p                           0/1     Completed   0          37m   10.36.0.10      kube-slave1   <none>

bankconsortiumbcnet   jcc-chall-hpflr                                        0/2     Completed   0          33m   10.40.0.12      kube-slave2   <none>

bankconsortiumbcnet   jcit-chsbihdfccc-golang-chsbihdfc-peer0-sbi-9dthw      0/1     Completed   0          28m   10.36.0.11      kube-slave1   <none>

bankconsortiumbcnet   jcitt-chsbihdfccc-golang-chsbihdfc-peer0-sbi-cbw24     0/1     Completed   0          27m   10.40.0.13      kube-slave2   <none>

bankconsortiumbcnet   juccf-pnb-k2sxf                                        0/1     Completed   0          34m   10.40.0.10      kube-slave2   <none>

bankconsortiumbcnet   juccf-sbi-peer1-pdnbv                                  0/1     Completed   0          38m   10.40.0.9       kube-slave2   <none>

bankconsortiumbcnet   jucf-chall-k8ftx                                       0/1     Completed   0          33m   10.36.0.11      kube-slave1   <none>

bankconsortiumbcnet   kafka0-bankconsortiumbcnet-com-5869c5bd7f-ndx4q        1/1     Running     0          50m   10.40.0.1       kube-slave2   <none>

bankconsortiumbcnet   kafka1-bankconsortiumbcnet-com-6fbd7f88-9p628          1/1     Running     0          50m   10.36.0.2       kube-slave1   <none>

bankconsortiumbcnet   kafka2-bankconsortiumbcnet-com-67bcd7749-gs7dp         1/1     Running     0          50m   10.40.0.2       kube-slave2   <none>

bankconsortiumbcnet   kafka3-bankconsortiumbcnet-com-568f855bfd-s7hkm        1/1     Running     0          50m   10.36.0.3       kube-slave1   <none>

bankconsortiumbcnet   orderer0-bankconsortiumbcnet-com-757f7b45dc-h78dl      1/1     Running     0          50m   10.40.0.3       kube-slave2   <none>

bankconsortiumbcnet   orderer1-bankconsortiumbcnet-com-6fdf675c5d-pr6t4      1/1     Running     0          50m   10.36.0.4       kube-slave1   <none>

bankconsortiumbcnet   peer0-hdfc-bankconsortiumbcnet-com-654f7bc676-mn65w    2/2     Running     0          50m   10.36.0.7       kube-slave1   <none>

bankconsortiumbcnet   peer0-icici-bankconsortiumbcnet-com-5d87d8696b-gm8dt   2/2     Running     0          50m   10.40.0.7       kube-slave2   <none>

bankconsortiumbcnet   peer0-pnb-bankconsortiumbcnet-com-6f7bb66c99-fmjkt     2/2     Running     0          34m   10.36.0.10      kube-slave1   <none>

bankconsortiumbcnet   peer0-sbi-bankconsortiumbcnet-com-cb96bbfc5-vddlv      2/2     Running     0          50m   10.36.0.6       kube-slave1   <none>

bankconsortiumbcnet   peer1-hdfc-bankconsortiumbcnet-com-7444947d97-ktchn    2/2     Running     0          50m   10.40.0.6       kube-slave2   <none>

bankconsortiumbcnet   peer1-icici-bankconsortiumbcnet-com-6795cdfc49-cdz7b   2/2     Running     0          50m   10.36.0.8       kube-slave1   <none>

bankconsortiumbcnet   peer1-pnb-bankconsortiumbcnet-com-67766f5f84-wf446     2/2     Running     0          34m   10.40.0.11      kube-slave2   <none>

bankconsortiumbcnet   peer1-sbi-bankconsortiumbcnet-com-8547c86bbb-shb9v     2/2     Running     0          38m   10.40.0.9       kube-slave2   <none>

bankconsortiumbcnet   zookeeper0-bankconsortiumbcnet-com-6f95986b98-7hjsh    1/1     Running     0          50m   10.36.0.0       kube-slave1   <none>

bankconsortiumbcnet   zookeeper1-bankconsortiumbcnet-com-59fbc47bf6-njnv6    1/1     Running     0          50m   10.36.0.1       kube-slave1   <none>

bankconsortiumbcnet   zookeeper2-bankconsortiumbcnet-com-7b789bcf46-g8d4s    1/1     Running     0          50m   10.40.0.0       kube-slave2   <none>

kube-system           coredns-576cbf47c7-2h8bp                               1/1     Running     0          54m   10.32.0.7       kube-master   <none>

kube-system           coredns-576cbf47c7-clnsk                               1/1     Running     0          54m   10.32.0.8       kube-master   <none>

kube-system           etcd-kube-master                                       1/1     Running     0          53m   172.16.10.161   kube-master   <none>

kube-system           kube-apiserver-kube-master                             1/1     Running     0          53m   172.16.10.161   kube-master   <none>

kube-system           kube-controller-manager-kube-master                    1/1     Running     0          53m   172.16.10.161   kube-master   <none>

kube-system           kube-proxy-4gk6j                                       1/1     Running     0          54m   172.16.10.161   kube-master   <none>

kube-system           kube-proxy-54vb4                                       1/1     Running     0          53m   172.16.10.148   kube-slave1   <none>

kube-system           kube-proxy-7n5rr                                       1/1     Running     0          53m   172.16.10.132   kube-slave2   <none>

kube-system           kube-scheduler-kube-master                             1/1     Running     0          53m   172.16.10.161   kube-master   <none>

kube-system           weave-net-7bp2d                                        2/2     Running     0          53m   172.16.10.148   kube-slave1   <none>

kube-system           weave-net-tw7pn                                        2/2     Running     0          53m   172.16.10.132   kube-slave2   <none>

kube-system           weave-net-v756w                                        2/2     Running     0          54m   172.16.10.161   kube-master   <none>


![alt text](https://github.com/piyushkumar96/bankconsortiumbcnet-multihost/blob/master/Screenshot%20ofallpodsrunning.png)
-----------------------------------------------------------------------------------------------------------------------------
                                                              END
-----------------------------------------------------------------------------------------------------------------------------
