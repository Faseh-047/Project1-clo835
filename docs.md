```sh
docker build -t frontend -f frontend/Dockerfile frontend/
docker build -t backend -f backend/Dockerfile backend/

docker network create app
docker run -p 27017:27017 --name mongo -d --network app mongo
docker run -d -p 8000:8000 -e PORT=8000 --name backend --network app -e MONGODB_URI=mongodb://mongo/database backend

minikube start
echo "" | docker login -u faseh047 --password-stdin
docker tag backend faseh047/backend
docker push faseh047/backend
docker tag frontend faseh047/frontend
docker push faseh047/frontend

kubectl apply -f namespace.yaml
kubectl apply -f secrets.yaml
kubectl apply -f configmaps.yaml
kubectl apply -f pv-and-pvc.yaml
kubectl apply -f db.yaml
kubectl apply -f backend.yaml
kubectl apply -f frontend.yaml

kubectl get deployments -n fullstack-app
kubectl get svc -n fullstack-app

