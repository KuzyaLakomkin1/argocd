# ---------------------------------- install ---------------------------------

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# ---------------------------------- get admin pass ---------------------------------

kubectl -n argocd get secrets argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d


# ---------------------------------- application.yaml ---------------------------------


---
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name     : myapp1
  namespace: argocd
  finalizers:
  - resources-finalizer.argocd.argoproj.io
spec:
  destination:
    name     : in-cluster
    namespace: app1
  source:
    path   : myapp
    repoURL: git@github.com:KuzyaLakomkin1/argocd.git
    targetRevision: HEAD
  project: default
  syncPolicy:
    automated:
      prune   : true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
