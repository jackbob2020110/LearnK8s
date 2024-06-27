sealos run registry.cn-shanghai.aliyuncs.com/labring/kubernetes:v1.27.7 registry.cn-shanghai.aliyuncs.com/labring/helm:v3.9.4 registry.cn-shanghai.aliyuncs.com/labring/cilium:v1.13.4 
    --masters 192.168.64.11
    --nodes 192.168.64.12,192.168.64.13 -p vagrant

#安装ingress-nginx,默认为deployment
sealos run registry.cn-shanghai.aliyuncs.com/labring/ingress-nginx:4.1.0

#执行下面步骤将deployment改为Daemonset

#先删除现有的deployment,然后执行kubectl apply -f ingress-controller-daemonset.yaml

需要给node打标签
kubectl label node 192.168.64.12 app=ingress
kubectl label node 192.168.64.13 app=ingress



kubectl apply -f nginx-demo.yaml

kubectl get po,svc,ingress
NAME                             READY   STATUS    RESTARTS   AGE
pod/new-nginx-8577765df7-fgl8f   2/2     Running   0          12m
pod/new-nginx-8577765df7-mpz5n   2/2     Running   0          12m

NAME                 TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1     `<none>`        443/TCP   143m
service/new-nginx    ClusterIP   10.96.0.177   `<none>`        80/TCP    12m

NAME                                  CLASS   HOSTS     ADDRESS      PORTS   AGE
ingress.networking.k8s.io/new-nginx   nginx   asun.io   10.96.1.30   80      12m
