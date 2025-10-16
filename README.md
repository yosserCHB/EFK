# EFK
Stack Elasticsearch, filbeat, and Kibana for log management with helm

⚙️ Installation de la stack EFK avec Helm
🧩 1. Ajouter les dépôts Helm nécessaires
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add elastic https://helm.elastic.co
helm repo update


🧱 2. Créer un namespace pour le logging
kubectl create namespace logging
(Créer les fichier .yaml )

🧠 3. Déployer Elasticsearch avec Helm
helm install elasticsearch elastic/elasticsearch -n logging -f elasticsearch-values.yaml

🔐 4. Récupérer le mot de passe utilisateur d’Elasticsearch
kubectl get secrets --namespace=logging elasticsearch-master-credentials -o jsonpath='{.data.password}' | base64 -d

🧪 5. Tester la santé du cluster Elasticsearch
kubectl exec -i -t elasticsearch-master-0 -n logging -- \
curl -k -u elastic:mot_de_passe_obtenu https://localhost:9200/_cluster/health?pretty


💠 6. Déployer Kibana avec Helm
helm install kibana elastic/kibana -n logging -f kibana-values.yaml

🔑 7. Récupérer les identifiants nécessaires
Mot de passe Elasticsearch :
kubectl get secrets --namespace=logging elasticsearch-master-credentials -o jsonpath='{.data.password}' | base64 -d
Token d’accès Kibana :
kubectl get secrets --namespace=logging kibana-kibana-es-token -o jsonpath='{.data.token}' | base64 -d

🧾 8. Lister tous les pods dans le namespace logging
kubectl get pods -n logging






