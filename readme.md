Benefits of Combining Tekton + ArgoCD

✅ Separation of Concerns

Tekton = builds and tests (ephemeral artifacts).

ArgoCD = deployment and long-term state management.

✅ End-to-End Automation

Code commit → tested → built → deployed without manual steps.   

✅ Scalability & Cloud-Native

Tekton tasks run as pods, scaling with cluster.

ArgoCD scales by managing multiple apps and clusters declaratively.

So ArgoCD + Tekton = Better GitOps

So tekton -> Tasks + pipelines
ArgoCD -> git repo + k8s cluster

So argoCD is a pull based Model and its declerative, and TEKTON is a push based model .
ArgoCD allows you to put you yaml files in a git repo and declare what you want to happen .
Use Tekton to the step of building and deploy an image and putting an image in a registry and then you can take all your config files put them in a git repo and all you got to do with argoCD is point at the git repo and point it a kubernetes cluster and its going to do the rest.


Install tekton  kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml

<img width="846" height="326" alt="image" src="https://github.com/user-attachments/assets/94b87d49-373d-4b0d-a29d-0fdac335589a" />

Check the pods 

<img width="568" height="92" alt="image" src="https://github.com/user-attachments/assets/5141fd8e-7f6d-494d-a15c-5fcb62dab781" />

Now install ArgoCD 

kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

<img width="885" height="237" alt="image" src="https://github.com/user-attachments/assets/64f8c5a2-f6ac-4d44-bb31-cda692644bdf" />

Check pods 

<img width="601" height="163" alt="image" src="https://github.com/user-attachments/assets/baf26dc1-6d1a-4cac-b148-b059c04b137b" />

expose Argocd via loadbalancer ->  kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'LoadBalancer"}}'

get the argocd Admin password -> kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decodede

user is : admin

we are on the Argo CD UI

<img width="1881" height="380" alt="image" src="https://github.com/user-attachments/assets/d9501b7f-93d7-4032-a76b-3f16f14be112" />

Install Tekton dashboard 

kubectl apply --filename https://storage.googleapis.com/tekton-releases/dashboard/latest/release.yaml

<img width="838" height="220" alt="image" src="https://github.com/user-attachments/assets/3f895f8f-b1e5-4230-b24e-4cb8522e5d9f" />

default port for Tekton dashboard is port: 9097

we are on the Tekton dashboard

<img width="1903" height="742" alt="image" src="https://github.com/user-attachments/assets/7d473345-7374-41c9-9d1d-5be986bfa79d" />


