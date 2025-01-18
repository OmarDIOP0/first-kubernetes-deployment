Voici un exemple de fichier `README.md` détaillé pour le déploiement de votre application sur **Kubernetes** avec **Minikube** et **kubectl**. Ce fichier inclut toutes les étapes nécessaires pour installer, configurer et déployer l'application.

---

# **Déploiement de l'application sur Kubernetes avec Minikube** 🚀

## **Description**
Ce guide explique comment déployer une application sur un cluster Kubernetes en utilisant **Minikube** et **kubectl**. Minikube est une solution simple qui permet de créer un cluster Kubernetes local pour le développement et le test.

---

## **Prérequis**
Avant de commencer, assurez-vous d'avoir installé les outils suivants sur votre machine :

1. **Minikube** : [Documentation d'installation](https://minikube.sigs.k8s.io/docs/start/)
2. **kubectl** : [Documentation d'installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
3. **Docker** : Pour créer et exécuter des conteneurs.

---

## **Étapes d'installation et de configuration**

### **1. Installer Minikube et kubectl**
- Téléchargez et installez **Minikube** et **kubectl** en suivant les liens ci-dessus.
- Assurez-vous que **Docker** est correctement installé et en cours d'exécution pour permettre la gestion des conteneurs.

---

### **2. Démarrer Minikube**
Lancez Minikube pour créer un cluster Kubernetes local :

```bash
minikube start --driver=docker
```

Cela crée un cluster Kubernetes local. Vous pouvez vérifier l'état du cluster avec la commande suivante :

```bash
minikube status
```
ou 

```bash
kubectl get nodes
```
Pour pouvez aussi avoir d'autres informations comme le service 

```bash	
kubectl get services
```
---

### **3. Créer une image Docker pour l'application**
Si vous n'avez pas encore d'image Docker pour votre application, créez-en une avec les étapes suivantes :

- **Étape 1** : Créez un fichier `Dockerfile` à la racine de votre projet. Exemple de fichier pour une application Node.js :

```dockerfile
# Utiliser une image Node.js de base
FROM node:14

# Définir le répertoire de travail
WORKDIR /app

# Copier les fichiers package.json et package-lock.json
COPY package*.json ./

# Installer les dépendances
RUN npm install

# Copier tout le projet dans le conteneur
COPY . .

# Exposer le port sur lequel l'application s'exécute
EXPOSE 3000

# Démarrer l'application
CMD ["npm", "start"]
```

- **Étape 2** : Construisez l'image Docker en utilisant Minikube comme environnement Docker :

```bash
eval $(minikube docker-env)  # Utiliser l'environnement Docker de Minikube
docker build -t mon-app:v1 .  # Construire l'image
```

---

### **4. Créer des fichiers de configuration Kubernetes**
Les fichiers de configuration Kubernetes définissent comment déployer votre application sur le cluster.

1. **Déploiement (deployment.yaml)** : Ce fichier décrit l'état souhaité pour votre application (nombre de réplicas, conteneurs à exécuter, etc.).

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mon-app-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: mon-app
  template:
    metadata:
      labels:
        app: mon-app
    spec:
      containers:
      - name: mon-app
        image: mon-app:v1
        ports:
        - containerPort: 3000
```

2. **Service (service.yaml)** : Ce fichier expose votre application en interne ou à l'extérieur du cluster.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mon-app-service
spec:
  type: NodePort
  selector:
    app: mon-app
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
      nodePort: 30001
```

---

### **5. Appliquer les fichiers de configuration à Kubernetes**
Utilisez la commande `kubectl apply` pour déployer votre application sur le cluster Minikube.

- **Déployer l'application** :

```bash
kubectl apply -f deployment.yaml
```

- **Exposer l'application via un service** :

```bash
kubectl apply -f service.yaml
```

- Vérifiez que les pods et services sont correctement créés :

```bash
kubectl get pods
kubectl get services
```

---

### **6. Accéder à l'application**
Minikube permet d'accéder à votre application via le service exposé en utilisant la commande suivante :

```bash
minikube service mon-app-service
```

Cela ouvrira une nouvelle fenêtre de votre navigateur avec l'application accessible via un port spécifique (dans cet exemple, le port **30001**).

---

### **7. Surveiller et gérer l'application**
Utilisez ces commandes pour surveiller et gérer l'état de l'application sur le cluster :

- **Voir les logs des pods** :
  ```bash
  kubectl logs <nom-du-pod>
  ```

- **Accéder à un pod pour débogage** :
  ```bash
  kubectl exec -it <nom-du-pod> -- /bin/bash
  ```

- **Supprimer les ressources Kubernetes** :
  Pour arrêter et supprimer l'application, vous pouvez supprimer le déploiement et le service créés :

  ```bash
  kubectl delete -f deployment.yaml
  kubectl delete -f service.yaml
  ```

---

### **8. Arrêter Minikube**
Lorsque vous avez terminé, vous pouvez arrêter Minikube avec la commande suivante :

```bash
minikube stop
```

Pour supprimer complètement le cluster, utilisez :

```bash
minikube delete
```

---

## **Conclusion**
Votre application est désormais déployée sur Kubernetes via Minikube. Vous pouvez continuer à itérer sur le code et redéployer les modifications en mettant à jour les fichiers YAML et en utilisant `kubectl apply`.

---

## **Références**
- [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)

---

## **Contributeurs**
- **Omar DIOP** (Développeur Fullstack)
