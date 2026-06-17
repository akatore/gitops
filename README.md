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
----
✦ Here is a summary of the steps we took to access Argo CD from Google Cloud Shell, including the technical reasons for each action. You can copy this into your README.md.

  ---
  
  Accessing Argo CD via Google Cloud Shell Web Preview                                   
  This guide outlines the steps required to securely access the Argo CD UI when running in a Kubernetes cluster accessed via Google Cloud Shell.                                                                                                                                                                                                                                                   
  1. Locate the Argo CD Service                                                                                                      
  First, we identified the service name and the namespace where Argo CD was installed.                                                                                                                                                                                                                                                                                                                    
   1 kubectl get svc -A | grep argocd-server                                                                                        
   * Why: We needed to confirm the service name (argocd-server) and namespace (argocd) to target the correct resource for port-forwarding.                                                                                                                    

  2. Initial Port-Forwarding Attempt (HTTPS)                                                                                                                                                                               
  Initially, we tried forwarding to the secure port (443).                                                                                                                                                                 
                                                                                                                                                                                                                           
   1 kubectl port-forward svc/argocd-server -n argocd 8080:443                                                                                                                                                             
   * Problem: This caused an ERR_TOO_MANY_REDIRECTS error.                                                                                                                                                                 
   * Why: Google Cloud Shell's Web Preview uses its own proxy to provide an HTTPS URL. When the backend (Argo CD) also tries to enforce HTTPS/TLS, the two proxies conflict, resulting in an infinite redirect loop.       
                                                                                                                                                                                                                           
  3. Enable Insecure Mode in Argo CD                                                                                                                                                                                       
  To resolve the redirect loop, we had to tell Argo CD to stop enforcing HTTPS internally, as the Cloud Shell proxy handles the encryption for us.                                                                         
                                                                                                                                                                                                                           
  Step A: Update the ConfigMap                                                                                                                                                                                             
                                                                                                                                                                                                                           
   1 kubectl patch cm argocd-cmd-params-cm -n argocd -p '{"data": {"server.insecure": "true"}}'                                                                                                                            
   * Why: Setting server.insecure: "true" allows the argocd-server to serve plain HTTP traffic on port 8080 (container port).                                                                                              
                                                                                                                                                                                                                           
  Step B: Restart the Deployment                                                                                                                                                                                           
                                                                                                                                                                                                                           
   1 kubectl rollout restart deploy argocd-server -n argocd                                                                                                                                                                
   2 kubectl rollout status deploy argocd-server -n argocd                                                                                                                                                                 
   * Why: Changes to the ConfigMap require a restart of the Argo CD server pods to take effect.                                                                                                                            
                                                                                                                                                                                                                           
  4. Final Port-Forwarding (HTTP)                                                                                                                                                                                          
  Now that Argo CD allows insecure connections, we forward the local Cloud Shell port 8080 to the service's HTTP port 80.                                                                                                  
                                                                                                                                                                                                                           
   1 kubectl port-forward svc/argocd-server -n argocd 8080:80                                                                                                                                                              
   * Why: This setup allows the Cloud Shell Web Preview to successfully proxy your request to the Argo CD UI without triggering redirect loops.                                                                            
                                                                                                                                                                                                                           
  5. Access the UI                                                                                                                                                                                                         
   1. Click the Web Preview button in the Cloud Shell toolbar.                                                                                                                                                             
   2. Select Preview on port 8080.                                                                                                                                                                                         
   3. Log in with the username admin.                                                                                                                                                                                      
                                                                                                                                                                                                                           
  6. Retrieve Initial Admin Password                                                                                                                                                                                       
  If you haven't changed it yet, get the auto-generated password:                                                                                                                                                          
                                                                                                                                                                                                                           
   1 kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d                                                                                                                   

```                                                                                                             
----


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
