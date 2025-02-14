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

### **3. Créer un deploiement kubernetes**


- : On va creer un deploiement Kubernetes pour executer des instances du serveur **Nginx**:
```bash
  kubectl create deployment nginx-depl --image=nginx
```	

- : Verifier le deployment:
```bash	
   kubectl get deployment
```	
- : Verifier le pod (Une abstraction qui encapsule un ou plusieurs conteneurs pour fonctionner ensemble comme une seule application)
```bash	
   kubectl get pod
```
- Pour debugger et voir les logs:
```bash	
   kubectl logs [pod_name] 
```
Pour acceder à l'interface bash du pod : [pod_name]:(kubectl get pod)
```bash	
   kubectl exec -it [pod_name] -- bin/bash
```
---

### **4. Appliquer le fichier de configuration à Kubernetes**
Utilisez la commande `kubectl apply` pour déployer votre application sur le cluster Minikube.

- **Déployer l'application** :

```bash
kubectl apply -f nginx-deployment.yaml
```
- **Verifier le deployment**
```bash	
 kubectl get deployment
```	
- **Chapitre 2: Deployement de MongoDB et MongoExpress** :

```bash
kubectl apply -f mongo-secret.yaml
```
- Ensuite

```bash
kubectl apply -f mongo-deployment.yaml
```
- Appliquer les services

```bash
kubectl apply -f mongo-service.yaml
```
- Vérifiez que les pods et services sont correctement créés :

```bash
kubectl get pods
kubectl get services
```
- Appliquer les configurations du mongo-express
```bash	
  kubectl apply -f mongo-config.yaml
```
- Appliquer le deployment de mongo-express
```bash	
 kubectl apply -f mongo-express-deployment.yaml
```
- Verifier la connexion du mongo-express et mongodb
```bash	
 kubectl logs [pods_name]
 ```
 - Creer le service mongo-express
 ```bash
 kubectl apply -f mongo-express-service.yaml
```

### **6. Accéder à l'application**
Minikube permet d'accéder à votre application via le service exposé en utilisant la commande suivante :

```bash
minikube service [service_name]
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
