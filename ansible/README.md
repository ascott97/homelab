# Ansible

Bootstrap a k3s cluster with argocd, which is used to deploy all other apps.

There is a one time action to retrieve the auto generated argocd admin password to log in & then delete this secret once the admin password has been updated.

1. Get the password  
  `kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`
2. Update the password via the UI  
  `kubectl -n argocd port-forward svc/argo-cd-argocd-server -n argocd 8080:80`
3. Delete the password secret  
  `kubectl -n argocd delete secret argocd-initial-admin-secret`

This will setup an initial argocd app to manage other apps, using the [app of apps pattern.](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#app-of-apps-pattern)  
It will use the `argocd/apps` directory in this repository.

## Setup

Create python virtual environment:  

`python3 -m venv venv`

Activate virtual environment:  

`source venv/bin/activate`

Install dependencies:  

`pip install -r requirements.txt`

To apply all:  

  `ansible-playbook playbooks/site.yml --ask-vault-pass`

