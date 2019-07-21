# Vagrant-k8s

Ansible playbook to create a k8s cluster in Google Cloud, using a vagrant file for testing and labs.

Create/change the playbook file with your configurations.

```yaml
- hosts: all
  connection: local
  gather_facts: yes
  vars:
    k8s_cluster: YOURCLUSTERNAME
    k8s_zone: us-central1-a
    kubectl_version: v1.14.0
    gcloud_project: YOURGCLOUDPROJECT
    node_type: n1-standard-4
    disk_size: 40
    node_count: 2
    install_ingress_controller: False
    install_istio: True
    istio_version: 1.1.4
    kiali_user: kiali
    kiali_pass: password
  roles:
    - { role: create-k8s-cluster }
    - { role: install-tools }
    - { role: install-istio }
```

Run your vagrant file and have your vm and k8s cluster ready.

```bash
vagrant up
```

to **destroy** your vm (remember to destroy your k8s cluster first)

1. Log into your vm

```bash
vagrant ssh
```

2. Run the ansible playbook to destroy your k8s Cluster

```bash
ansible-playbook /vagrant/playbook-destroy.yml -i /vagrant/hosts
```

3. Destroy the vm

```bash
vagrant destroy -f
```

**Notes**:

You must create an account previously and write the associated key to roles/create-k8s-cluster/files/gcp-service-account.json, this account must have enough privileges to create a cluster in your gcloud project.

For your GCloud project (Example for this lab is 'gdcdevops')

This account can be created in

    IAM & admin
    Service Accounts
    Create Service Account
        Role: Project/Owner

Then create key in json format and write it to roles/create-k8s-cluster/files/gcp-service-account.json
