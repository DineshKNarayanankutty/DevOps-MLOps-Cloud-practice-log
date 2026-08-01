# Kubernetes Level 1 Certification Practice Log

**Date:** 01/08/2026
**Platform:** KodeKloud
**Domain:** Kubernetes (Level 1)
**Topics Covered:** Pods, ReplicaSets, Deployments, Services, CronJobs, Kubernetes Troubleshooting

---

# Task 1: Create a Pod with Labels

### Objective

Create a Pod named **httpd-test-t1q4** using the **httpd:alpine3.19** image and assign the label `app=httpd`.

### Command Used

```bash
kubectl run httpd-test-t1q4 \
  --image=httpd:alpine3.19 \
  --labels=app=httpd
```

### Verification

```bash
kubectl get pods --show-labels
kubectl describe pod httpd-test-t1q4
```

### Key Learning

* Labels can be assigned directly while creating a Pod using `--labels`.
* YAML syntax (`app: httpd`) is equivalent to CLI syntax (`app=httpd`).

---

# Task 2: Create HTTPD Pod with Custom Container Name

### Objective

Create a Pod with:

* Pod Name: `pod-httpd-t1q1`
* Image: `httpd:latest`
* Label: `app=httpd_app_t1q1`
* Container Name: `httpd-container-t1q1`

### YAML Used

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-httpd-t1q1
  labels:
    app: httpd_app_t1q1
spec:
  containers:
  - name: httpd-container-t1q1
    image: httpd:latest
```

### Apply

```bash
kubectl apply -f pod.yaml
```

### Verification

```bash
kubectl get pods
kubectl describe pod pod-httpd-t1q1
```

### Key Learning

* `kubectl run` cannot specify a custom container name.
* YAML manifests are preferred whenever container-level customization is required.

---

# Task 3: Rollback a Deployment

### Objective

Rollback deployment:

```
nginx-deployment-t2q2
```

to its previous revision.

### Command Used

```bash
kubectl rollout undo deployment/nginx-deployment-t2q2
```

### Verification

```bash
kubectl rollout history deployment/nginx-deployment-t2q2
kubectl rollout status deployment/nginx-deployment-t2q2
```

### Key Learning

* Deployment revisions allow quick rollback to a previously working version.

---

# Task 4: Scale a Deployment

### Objective

Increase replicas from **1 → 3**.

Deployment:

```
blue-app-t2q5
```

### Command Used

```bash
kubectl scale deployment blue-app-t2q5 --replicas=3
```

### Verification

```bash
kubectl get deployment blue-app-t2q5
kubectl get pods
```

### Key Learning

* Scaling only changes the replica count without modifying the application configuration.

---

# Task 5: Create a ReplicaSet

### Objective

Create ReplicaSet:

```
httpd-replicaset-t3q5
```

Requirements:

* Image: `httpd:latest`
* Replicas: 3
* Label: `app=httpd-t3q5`

### YAML Used

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: httpd-replicaset-t3q5
spec:
  replicas: 3
  selector:
    matchLabels:
      app: httpd-t3q5
  template:
    metadata:
      labels:
        app: httpd-t3q5
    spec:
      containers:
      - name: httpd
        image: httpd:latest
```

### Apply

```bash
kubectl apply -f replicaset.yaml
```

### Verification

```bash
kubectl get rs
kubectl get pods --show-labels
```

### Key Learning

* ReplicaSets ensure the desired number of Pods remain running.
* Pod labels and ReplicaSet selectors must match exactly.

---

# Task 6: Create a CronJob

### Objective

Create CronJob:

```
devops-t3q1
```

Requirements:

* Schedule: `*/9 * * * *`
* Image: `httpd:latest`
* Restart Policy: `OnFailure`

### YAML Used

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: devops-t3q1
spec:
  schedule: "*/9 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: OnFailure
          containers:
          - name: cron-devops-t3q1
            image: httpd:latest
            command:
            - /bin/sh
            - -c
            - echo Welcome to xfusioncorp!
```

### Apply

```bash
kubectl apply -f cronjob.yaml
```

### Verification

```bash
kubectl get cronjobs
kubectl describe cronjob devops-t3q1
```

### Key Learning

* CronJobs execute Jobs based on a defined schedule.
* `restartPolicy: OnFailure` restarts failed Jobs only.

---

# Task 7: Troubleshoot Service Selector Issue

### Problem

Application:

```
orange-app-deployment-t4q6
```

was inaccessible.

### Investigation Commands

```bash
kubectl describe deployment orange-app-deployment-t4q6

kubectl describe pod <pod-name>

kubectl describe svc orange-app-service-t4q6

kubectl get endpoints orange-app-service-t4q6
```

### Root Cause

Service selector contained a typo.

Incorrect:

```yaml
selector:
  app: orage-app-t4q6
```

Correct:

```yaml
selector:
  app: orange-app-t4q6
```

Because of this typo:

```bash
kubectl get endpoints orange-app-service-t4q6
```

returned:

```
<none>
```

### Fix

```bash
kubectl edit svc orange-app-service-t4q6
```

Updated the selector to match the Pod label.

### Verification

```bash
kubectl get endpoints orange-app-service-t4q6
kubectl describe svc orange-app-service-t4q6
```

### Key Learning

* A Service routes traffic only to Pods whose labels match its selector.
* Even a single-character typo results in zero endpoints and an inaccessible application.

---

# Task 8: Troubleshoot Flask Application

### Problem

Deployment:

```
python-deployment-devops-t4q4
```

was inaccessible.

### Investigation

```bash
kubectl describe deployment python-deployment-devops-t4q4

kubectl describe svc python-service-devops-t4q4

kubectl get pods
```

### Issues Identified

#### Issue 1: Incorrect Image

Incorrect:

```
poroko/flask-app-demo
```

Correct:

```
poroko/flask-demo-app
```

#### Issue 2: Incorrect Target Port

Incorrect target port prevented traffic from reaching the application.

Correct configuration:

```yaml
targetPort: 5000
nodePort: 32345
```

### Fix

```bash
kubectl edit deployment python-deployment-devops-t4q4

kubectl edit svc python-service-devops-t4q4
```

Updated:

* Image → `poroko/flask-demo-app`
* `targetPort` → `5000`

### Verification

```bash
kubectl rollout status deployment/python-deployment-devops-t4q4

kubectl get svc

kubectl get endpoints
```

### Key Learning

* Flask applications expose port **5000** by default.
* Service `targetPort` must match the application's listening port.
* Incorrect image names result in deployment failures.

---

# Task 9: Create a ClusterIP Service

### Objective

Expose deployment:

```
deployment-t5q2
```

using a ClusterIP Service.

Requirements:

* Service Name: `deployment-svc-t5q2`
* Port: `8090`
* Target Port: `80`

### Command Used

```bash
kubectl expose deployment deployment-t5q2 \
  --name=deployment-svc-t5q2 \
  --type=ClusterIP \
  --port=8090 \
  --target-port=80
```

### Verification

```bash
kubectl get svc deployment-svc-t5q2

kubectl describe svc deployment-svc-t5q2
```

### Key Learning

* ClusterIP is the default Service type used for internal communication within the Kubernetes cluster.

---

# Task 10: Update Service Target Port

### Objective

Update Service:

```
service-t5q4
```

to use:

```
targetPort: 80
```

### Command Used

```bash
kubectl edit svc service-t5q4
```

Modified:

```yaml
targetPort: 80
```

### Verification

```bash
kubectl describe svc service-t5q4
```

### Key Learning

* `targetPort` specifies the port on the container where traffic is forwarded.
* If `targetPort` is incorrect, the Service cannot communicate with the application.

---

# Commands Practiced Today

```bash
kubectl run
kubectl apply
kubectl get
kubectl describe
kubectl edit
kubectl expose
kubectl rollout undo
kubectl rollout history
kubectl rollout status
kubectl scale
kubectl get endpoints
kubectl logs
kubectl set image
```

---

# Kubernetes Concepts Revised

* Pod creation
* Labels and Selectors
* Container naming
* ReplicaSets
* Deployment rollback
* Deployment scaling
* CronJobs
* ClusterIP Services
* NodePort Services
* Service selectors
* Endpoints
* TargetPort vs Port vs NodePort
* Troubleshooting Deployments
* Troubleshooting Services
* Image validation
* YAML resource definitions