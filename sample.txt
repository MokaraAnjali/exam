---------------------------------------------------------------------------------EX1 :

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>hello docker</h1>
</body>
</html>


<!-- git init 
git add . 
git commit -m ""
git branch -m main 
git remote add origin http://github.com/MokaraAnjali/exam
git push -u origin main 

git config --global user.name "Your Name" 
git config --global user.email "youremail@example.com"  -->

-----------------------------------------------------------------------------------EX2

FROM nginx 
COPY index.html /usr/share/nginx/html/index.html


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>hello docker</h1>
</body>
</html>



<!-- docker build -t ex . 
docker run -d -p 8080:80 ex -->


------------------------------------------------------------------------------------EX3

FROM python:3.9-slim
WORKDIR / app
COPY requirements.txt .
RUN pip install -r requirements.txt 
COPY . .
EXPOSE 5000
CMD ["python","app"]


Flask

from flask import Flask,render_template_string
app=Flask(__name__)

HTML_TEMPLATE ='''

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>hello docker</h1>
</body>
</html>

'''

@app.route('/')
def home():
    return render_template_string(HTML_TEMPLATE)

if __name__ =="__main__":
    app.run(host='0.0.0.0',port=5000)


docker build -t flask-docker-app . 
docker run -p 5000:5000 flask-docker-app
docker login 
docker tag flask-docker-app yourname/flask-docker-app:latest 
docker push anjalimokara/flask-docker-app:latest

docker pull anjalimokara/flask-docker-app:latest 
docker run -p 5000:5000 yourname/flask-docker-app:latest

----------------------------------------------ex4

docker-compose.yml

version: '3.8'
services:
  jenkins:
    image:  jenkins/jenkins:lts 
    ports: 
    - "8080:8080" 
    - "50000:50000" 
    volumes: 
    - jenkins_home:/var/jenkins_home 
volumes: 
  jenkins_home: 


  docker compose up -d 
  docker ps
  docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword(copy password) 
  docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword

 Install Suggested Plugins 
o Create admin user 
2. Install “HTML Publisher” plugin 
o Go to: Manage Jenkins → Plugins → Available Plugins → Search “HTML 
Publisher” → Install 
3. Create a Freestyle Job (Simpler than Pipeline) 
o Click New Item → Freestyle project 
o Name: simple-html-site 
o In Build Steps: 
▪ Add “Execute shell” step 
▪ Paste this simple command: 
▪ mkdir site 
▪ echo "<h1>Welcome to Jenkins Static Site</h1>" > site/index.html 
o In Post-build Actions: 
▪ Add “Publish HTML reports” 
▪ Directory: site 
▪ Index page: index.html 
▪ Report title: Simple HTML Site 
4. Click Build Now 
o After it finishes, open build → click Simple HTML Site on left side. 

  -----------------------------------------------------ex5 
  
app.py 
dockerfile 
FROM python:3.11 
WORKDIR /app 
COPY . . 
CMD ["python", "app.py"]

  
docker build -t anjalimokara/demo:local . 
docker run anjalimokara/demo:local 
docker login 
docker push anjalimokara/demo:local

name: Docker Build & Push 
 
on: [push] 
 
jobs: 
  build: 
    runs-on: ubuntu-latest 
    steps: 
      - uses: actions/checkout@v4 
      - uses: docker/login-action@v3 
        with: 
          username: ${{ secrets.DOCKERHUB_USERNAME }} 
          password: ${{ secrets.DOCKERHUB_TOKEN }} 
      - uses: docker/build-push-action@v6
        with: 
          push: true 
          tags: anjalimokara/demo:latest 

git init 
git add . 
git commit -m "initial commit" 
git branch -M main 
git remote add origin https://github.com/MokaraAnjali/demo.git 
git push -u origin main 

Push any code change: 
git add . 
git commit -m "update" 
git push 
(create token in Docker Hub → Account Settings → Security → 
New Access Token) 
Then go to your GitHub repo → Actions tab → see the workflow run. 
After success, check Docker Hub → 
docker build -t anjalimokara/demo:local . 
docker push anjalimokara/demo:local



-----------------------------------------------------------EX6
minikube start --driver=docker
kubectl get nodes 

nginx-pod.yaml 

apiVersion: v1 
kind: Pod 
metadata: 
  name: nginx-pod 
spec: 
  containers: 
  - name: nginx 
    image: nginx:latest 
    ports: 
      - containerPort: 80



kubectl apply -f nginx-pod.yaml 
kubectl get pods 
kubectl describe pod nginx-pod 
kubectl port-forward pod/nginx-pod 8080:80 
Meaning: You can open http://localhost:8080 in browser. 
Create deployment 
kubectl create deployment my-nginx --image=nginx 
Scale deployment 
kubectl scale deployment my-nginx --replicas=3 
kubectl get pods 
kubectl set image deployment/my-nginx nginx=nginx:1.25 
kubectl expose deployment my-nginx --type=NodePort --port=80
kubectl get svc 
minikube service my-nginx –url



-----------------------------------------------------------------ex7
app.py 

from flask import Flask 
app = Flask(__name__) 
@app.route('/') 
def home(): 
return '<p>hello flask</p>' 
if __name__ == '__main__': 
app.run(host='0.0.0.0', port=5000) 

requirements.txt 
flask 

Dockerfile 
FROM python:3.9-slim 
WORKDIR /app 
COPY . /app 
RUN pip install -r requirements.txt 
EXPOSE 5000 
CMD ["python", "app.py"]


docker build -t anjalimokara/hello-flask . 
docker run -p 5000:5000 anjalimokara/hello-flask 
Open browser → http://localhost:5000 
Login and push (only once): 
docker login 
docker push anjalimokara/hello-flask


hello-flask.yaml 

apiVersion: apps/v1 
kind: Deployment 
metadata: 
  name: hello-flask 
spec: 
  replicas: 1 
  selector: 
    matchLabels: 
      app: flask 
  template: 
    metadata: 
      labels: 
        app: flask 
    spec: 
      containers: 
      - name: flask 
        image: beereddy2004/hello-flask 
        ports: 
        - containerPort: 5000
        
--- 
apiVersion: v1 
kind: Service 
metadata: 
  name: flask-svc 
spec: 
  type: NodePort 
  selector: 
app: flask 
ports: - port: 5000 
targetPort: 5000 




Deploy on Minikube 
minikube start --driver=docker 
kubectl apply -f hello-flask.yaml 
kubectl get all 
Access the App 
minikube service flask-svc --url
