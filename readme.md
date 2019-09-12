# Home Lab

I am using a I7 machine with 64G of ram and a 600G disk and I connect to my openwrt where I use dnsmask to simulate A records
on my box machine I use the xen server [XCP-NG](https://xcp-ng.org) what is a greate aquesition and it brings [XOA](https://xen-orchestra.com/#!/xosan-home) Great job!
or KVM and use vagrant to deployment
you can clone my repo [Orpere](https://github.com/orpere) and [kubespray](https://github.com/kubernetes-sigs/kubespray) inside what will help with the kubernetes build.

1 - create a themplate in xen with your election distro "I have add ubuntu18.04 and centos7"
    and don't forget to add your public key to the instances and enable sudo to your user. I use NOPASSWD

2 - run kubespray against your instances and don't forget to add the option --user and --private-key to your ansible-playbook command

```bash
    ansible-playbook  -i inventory/mycluster/hosts.yml --user=<YOUR USER> --become --become-user=root cluster.yml --private-key=<YOUR KEY>
```

NOTE: this will take long time please have a coffe :)

3 - get your [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) working it will help to manage the cluster form your network

3.1 - Copy from the kubespray host the file on /etc/kubernetes/admin.conf  to your local ~/.kube/config
3.2 - kubectl cluster-info
          should return:

```bash
            Kubernetes master is running at https://<api_ip>:6443
            coredns is running at https://<api_ip>:6443/api/v1/namespaces/kube-system/services/coredns:dns/proxy
            kubernetes-dashboard is running at https://<api_ip>:6443/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy
```

3.3 - kubectl config current-context (for check what cluster is your config ) should return something as: kubernetes-admin@cluster.local

4 - get metallb working

```bash
kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.8.1/manifests/metallb.yaml
kubectl apply -f k8s-cluster-manifests/metallb-map.yaml
```

5 - install traefik

```bash
helm init --wait
kubectl create serviceaccount --namespace kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
helm install stable/traefik --name traefik --set dashboard.enabled=true,serviceType=NodePort,dashboard.domain=dashboard.traefik,rbac.enabled=true  --namespace kube-system

#traefik dashboard with password 
note: first need to remove the previous deployment
htpasswd -nb traefik password


helm install stable/traefik --name traefik --set dashboard.enabled=true,serviceType=LoadBalancer,dashboard.domain=traefik.lan,rbac.enabled=true,dashboard.auth.basic.traefik='$apr1$npha/qF1$VD51O1swfgGWmuiaDfdZA0' --namespace kube-system

Note: check the service type and password
```

6 - create a dashboard user admin

```bash
kubectl apply -f k8s-cluster-manifests/cluster-admin.yaml
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | (grep admin-user || echo "$_") | awk '{print $1}') | grep token: | awk '{print $2}'
```
