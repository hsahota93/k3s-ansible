# k3s-ansible

1. Update apt on all nodes `ansible-playbook ./playbooks/apt.yaml -i ./inventory/k3s-cluster/hosts.ini --ask-pass --ask-become-pass`
2. Set timezone on all nodes `ansible-playbook ./playbooks/timezone.yaml -i ./inventory/k3s-cluster/hosts.ini --ask-pass --ask-become-pass`
3. Install k3s by running: `ansible-playbook ./site.yml -i ./inventory/k3s-cluster/hosts.ini --ask-pass --ask-become-pass`
    3a. For some fucking reason I have to run this: 
    ```
    helm -n kube-system delete traefik traefik-crd
    kubectl -n kube-system delete helmchart traefik traefik-crd
    touch /var/lib/rancher/k3s/server/manifests/traefik.yaml.skip
    kubectl delete svc traefik -n kube-system
    systemctl restart k3s
    ```
    From: https://github.com/k3s-io/k3s/issues/1160 
4. Install Traefik: `ansible-playbook ./playbooks/traefik/traefik.yaml -i ./inventory/k3s-cluster/hosts.ini --ask-pass --ask-become-pass`

Followed these guides:
* https://technotim.live/posts/k3s-etcd-ansible/
* https://technotim.live/posts/kube-traefik-cert-manager-le/