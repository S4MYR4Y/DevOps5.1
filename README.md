# kubeplugin — kubectl extension

Простий плагін для `kubectl`, який показує статистику CPU та пам’яті для ресурсів Kubernetes (pods, nodes тощо).  
Написаний на **bash** для зручності та простоти.

---

## 📦 Встановлення

1. Клонуйте репозиторій:
   ```bash
   git clone https://github.com/S4MYR4Y/devops5.1.git
   cd devops5.1
   ```
2. Зробіть плагін виконуваним:
    ``` bash
    chmod +x scripts/kubectl-kubeplugin
    ```
3. Додайте директорію scripts у PATH (наприклад у ~/.zshrc або ~/.bashrc):
    ``` bash
    export PATH=$PATH:$(pwd)/scripts
    ```
4. Запустіть кластер:
    ```bash
    kind create cluster --name devops51
    ```
5. Потрібно встановити metrics-server, інакше kubectl top не працюватиме.
    ```bash 
    kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

kubectl patch deployment metrics-server -n kube-system \
  --type='json' -p='[{"op": "add", "path": "/spec/template/spec/containers/0/args/-", "value":"--kubelet-insecure-tls"}]'
    ```

6. Перевірте, що metrics-server працює:
    ```bash
    kubectl -n kube-system get pods | grep metrics-server

metrics-server-56675bc7d7-98wrg                  0/1     Running   0          5s
metrics-server-6c996746c8-wk4n6                  1/1     Running   0          83s
    ```

7. Запустіть bash-скрипт:

```bash
 kubectl kubeplugin kube-system pods


Resource, Namespace, Name, CPU, Memory
pods, kube-system, coredns-668d6bf9bc-mx744, 3m, 13Mi
pods, kube-system, coredns-668d6bf9bc-n5kb7, 5m, 13Mi
pods, kube-system, etcd-devops51-control-plane, 34m, 29Mi
pods, kube-system, kindnet-kkzjm, 1m, 10Mi
pods, kube-system, kindnet-l84zb, 1m, 14Mi
pods, kube-system, kindnet-lthlw, 1m, 14Mi
pods, kube-system, kindnet-zv9h9, 2m, 14Mi
pods, kube-system, kube-apiserver-devops51-control-plane, 57m, 223Mi
pods, kube-system, kube-controller-manager-devops51-control-plane, 28m, 64Mi
pods, kube-system, kube-proxy-287tl, 2m, 16Mi
pods, kube-system, kube-proxy-lvscb, 1m, 13Mi
pods, kube-system, kube-proxy-nqvvf, 1m, 15Mi
pods, kube-system, kube-proxy-tcr5j, 1m, 17Mi
pods, kube-system, kube-scheduler-devops51-control-plane, 13m, 25Mi
pods, kube-system, metrics-server-56675bc7d7-98wrg, 8m, 17Mi
```
