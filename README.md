# Kubernetes Lab: ConfigMaps, Secrets, Pods, and Persistent Storage

## Lab Overview

This lab provides **hands-on practice** with Kubernetes core concepts:

* **ConfigMaps** → Manage application configurations.
* **Secrets** → Store sensitive data securely.
* **Pods** → Run single or multi-container workloads.
* **InitContainers** → Run pre-processing tasks.
* **Environment Variables** → Dynamically configure containers.
* **Persistent Volumes & Claims** → Manage persistent storage.

**Goal:** Learn to configure applications dynamically, manage secrets, and persist data in Kubernetes.

---

## Lab Objectives

| Objective             | Description                                                   |
| --------------------- | ------------------------------------------------------------- |
| ConfigMaps            | Create, inspect, and use ConfigMaps in Pods.                  |
| Secrets               | Create Secrets and inject into Pods as environment variables. |
| Pods                  | Deploy single/multi-container Pods with proper configuration. |
| InitContainers        | Run tasks before main container starts.                       |
| Environment Variables | Configure Pods to print and use dynamic variables.            |
| Persistent Storage    | Create PV and PVC, attach them to Pods.                       |

---

## 🔹 Lab Tasks

### 1️⃣ ConfigMaps

**Steps:**

1. List existing ConfigMaps:

```bash
kubectl get configmaps
```

2. Create `webapp-config-map` with `APP_COLOR`.
3. Deploy Pod `webapp-color` consuming this ConfigMap.

---

### 2️⃣ Secrets

**Steps:**

1. List all Secrets: `kubectl get secrets`
2. Inspect `default-token` Secret: `kubectl describe secret <secret-name>`
3. Create `db-secret` with MySQL credentials.
4. Deploy `db-pod` using `db-secret` for environment variables.
5. Observe Pod readiness issues (e.g., initialization or storage pending).

---

### 3️⃣ Multi-Container Pod

**Pod Name:** `yellow`
**Containers:**

| Container | Image   |
| --------- | ------- |
| lemon     | busybox |
| gold      | redis   |

* Ensure both containers run simultaneously.

---

### 4️⃣ Init Container Pod

**Pod Name:** `red`
**Configuration:**

* `initContainer`: busybox, sleeps 20s
* Main container: redis

 Purpose: Delay main container until pre-task finishes.

---

### 5️⃣ Environment Variables

**Pod Name:** `print-envars-greeting`
**Container:** `print-env-container`
**Environment Variables:**

| Name     | Value      |
| -------- | ---------- |
| GREETING | Welcome to |
| COMPANY  | DevOps     |
| GROUP    | Industries |

**Command:** Echo the full message:

```
$GREETING $COMPANY $GROUP
```

---

### 6️⃣ Kubeconfig Exploration

* Default location: `~/.kube/config`
* Check clusters: `kubectl config view`
* Current user in context: `kubectl config current-context`

---

### 7️⃣ Persistent Volumes & Claims

1. Create PV `pv-log` with 100Mi and `ReadWriteMany`.
2. Create PVC `claim-log-1` requesting 50Mi.
3. Deploy Pod `webapp` mounting PVC at `/var/log/nginx`.

**Diagram:**

```
[webapp Pod]
     │
     │ mounts
     ▼
[PersistentVolumeClaim: claim-log-1]
     │
     │ bound to
     ▼
[PersistentVolume: pv-log]
     │
     │ storage at
     ▼
  /pv/log (hostPath)
```

---

##  Lab Workflow

1. Apply resources in order: ConfigMaps → Secrets → Pods → Storage.
2. Inspect resources:

```bash
kubectl get pods
kubectl get configmaps
kubectl get secrets
kubectl get pvc
kubectl get pv
```

3. Check logs for environment variable verification:

```bash
kubectl logs -f <pod-name>
```

4. Monitor Pod status and troubleshoot if necessary.

---

##  Expected Outcomes

* ConfigMaps and Secrets successfully created and used by Pods.
* Multi-container Pods running correctly.
* InitContainers executing as expected.
* Environment variables printed properly.
* Persistent storage attached and usable by Pods.

---

##  Commands Reference

| Action            | Command                              |
| ----------------- | ------------------------------------ |
| Apply resources   | `kubectl apply -f <folder-or-file>`  |
| List Pods         | `kubectl get pods`                   |
| List ConfigMaps   | `kubectl get configmaps`             |
| List Secrets      | `kubectl get secrets`                |
| Inspect resources | `kubectl describe <resource> <name>` |
| Check logs        | `kubectl logs -f <pod-name>`         |
| Current context   | `kubectl config current-context`     |
