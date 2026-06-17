# gitops

set up:
```
https://shell.cloud.google.com/?pli=1&show=terminal
```
```
# Pick/create a project if needed
gcloud config set project YOUR_PROJECT_ID

# Start a local cluster in Cloud Shell
minikube start --driver=docker

# Confirm it works
kubectl get nodes

kubectl create namespace argocd
kubectl apply -n argocd --server-side --force-conflicts -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

```
```shell
✦ Since you are using Google Cloud Shell, you can access the Argo CD UI using the Web Preview feature:                                                                                                                     
                                                                                                                                                                                                                           
   1. Click the Web Preview button in the top right of the Cloud Shell taskbar.                                                                                                                                            
   2. Select Preview on port 8080.                                                                                                                                                                                         
                                                                                                                                                                                                                           
  Cloud Shell will open a new browser tab with a URL like https://8080-dot-xxxxxxx-dot-devshell.dev.                                                                                                                       
                                                                                                                                                                                                                           
  Note on TLS: Since the port-forward is mapping to the HTTPS port (443), your browser might show a certificate warning because the Argo CD self-signed certificate doesn't match the devshell.dev domain. You can usually 
  click "Advanced" and "Proceed to localhost (unsafe)" to continue.                                                                                                                                                        
                                                                                                                                                                                                                           
  If you'd rather use HTTP to avoid the certificate warning, I can restart the forward on port 80:                                                                                                                         
                                                                                                                                                                                                                           
   1 kubectl port-forward svc/argocd-server -n argocd 8080:80                                                                                                                                                              
                                                                                                                                                                                                                           
  Would you like me to switch to port 80?                                                                                                                                                                                  
▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
 > This page isn’t working                                                                                                                                                                                                 
   8080-cs-86f6f21c-4095-438f-a410-11f2387e0a0e.cs-asia-southeast1-yelo.cloudshell.dev redirected you too many times.                                                                                                      
                                                                                                                                                                                                                           
   Try deleting your cookies.                                                                                                                                                                                              
   ERR_TOO_MANY_REDIRECTS                                                                                                                                                                                                 
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮     
│ ✓  Shell kubectl patch cm argocd-cmd-params-cm -n argocd -p '{"data": {"server.insecure": "true"}}' && kubectl rollout restart deploy argocd-server -n argocd                                                      │     
│                                                                                                                                                                                                                    │     
│ configmap/argocd-cmd-params-cm patched                                                                                                                                                                             │     
│ deployment.apps/argocd-server restarted                                                                                                                                                                            │     
│                                                                                                                                                                                                                    │     
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯     
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮     
│ ✓  Shell kubectl rollout status deploy argocd-server -n argocd                                                                                                                                                     │     
│                                                                                                                                                                                                                    │     
│ Waiting for deployment "argocd-server" rollout to finish: 1 old replicas are pending termination...                                                                                                                │     
│ Waiting for deployment "argocd-server" rollout to finish: 1 old replicas are pending termination...                                                                                                                │     
│ deployment "argocd-server" successfully rolled out                                                                                                                                                                 │     
│                                                                                                                                                                                                                    │     
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯     
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮     
│ ✓  Shell kill 9552 && kubectl port-forward svc/argocd-server -n argocd 8080:80                                                                                                                                     │     
│                                                                                                                                                                                                                    │     
│ Command moved to background (PID: 10202). Output hidden. Press Ctrl+B to view.                                                                                                                                     │     
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯     
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮     
│ ✓  Read Background Output Reading output for background process 10202                                                                                                                                              │     
│                                                                                                                                                                                                                    │     
│ Full Log Output:                                                                                                                                                                                                   │     
                                                                                                                                                                            │                                        │     
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯     
                                                                                                                                                                                                                           
✦ The redirect loop was caused by Argo CD trying to force HTTPS internally, which conflicted with the Cloud Shell proxy.                                                                                                   
                                                                                                                                                                                                                           
  I have:                                                                                                                                                                                                                  
   1. Updated the Argo CD configuration (argocd-cmd-params-cm) to enable insecure mode.                                                                                                                                    
   2. Restarted the argocd-server deployment to apply the change.                                                                                                                                                          
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮     
│ ✓  Shell kubectl patch cm argocd-cmd-params-cm -n argocd -p '{"data": {"server.insecure": "true"}}' && kubectl rollout restart deploy argocd-server -n argocd                                                      │     
│                                                                                                                                                                                                                    │     
│ configmap/argocd-cmd-params-cm patched                                                                                                                                                                             │     
│ deployment.apps/argocd-server restarted                                                                                                                                                                            │     
│                                                                                                                                                                                                                    │     
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯     
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮     
│ ✓  Shell kubectl rollout status deploy argocd-server -n argocd                                                                                                                                                     │     
│                                                                                                                                                                                                                    │     
│ Waiting for deployment "argocd-server" rollout to finish: 1 old replicas are pending termination...                                                                                                                │     
│ Waiting for deployment "argocd-server" rollout to finish: 1 old replicas are pending termination...                                                                                                                │     
│ deployment "argocd-server" successfully rolled out                                                                                                                                                                 │     
│                                                                                                                                                                                                                    │     
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯     
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮     
│ ✓  Shell kill 9552 && kubectl port-forward svc/argocd-server -n argocd 8080:80                                                                                                                                     │     
│                                                                                                                                                                                                                    │     
│ Command moved to background (PID: 10202). Output hidden. Press Ctrl+B to view.                                                                                                                                     │     
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯     
╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮     
│ ✓  Read Background Output Reading output for background process 10202                                                                                                                                              │     
│                                                                                                                                                                                                                    │     
│ Full Log Output:                                                                                                                                                                                                   │     
                                                                                                                                                                            │     
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯     
                                                                                                                                                                                                                           
✦ The redirect loop was caused by Argo CD trying to force HTTPS internally, which conflicted with the Cloud Shell proxy.                                                                                                   

  I have:
   1. Updated the Argo CD configuration (argocd-cmd-params-cm) to enable insecure mode.
   2. Restarted the argocd-server deployment to apply the change.
   3. Restarted the port-forward on port 8080.

  Please try the Web Preview again:
   1. Close your previous Argo CD browser tab.
   2. Click Web Preview -> Preview on port 8080.

  It should now load correctly without any redirects!
```
