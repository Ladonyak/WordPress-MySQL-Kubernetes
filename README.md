# WordPress-MySQL-Kubernetes

apiVersion: apps/v1
# Объявить объект ресурса развертывания
kind: Deployment
metadata:
  name: deployment-myapp
spec:
 # Объявляем количество контейнеров 2 через реплики  
  replicas: 2
 # Выберите управляемый модуль по тегу    
  selector:
    matchLabels:
      app: myapp
 # Определить под в шаблоне      
  template:
    metadata:
 # Отметить pod app = myapp    
      labels:
        app: myapp
    spec:
      containers:
 # Объявить имя контейнера, обратите внимание, что это не имя модуля, имя модуля должно быть определено в метаданных
      - name: myapp
        image: ikubernetes/myapp:v1
        ports:
        - name: http
          containerPort: 80
 # Разделить несколько объектов ресурсов в файле yaml через ---          
---
apiVersion: v1
 # Объявить объект ресурса службы
kind: Service
metadata:
  name: service-myapp
spec:
 # service-myapp выберет модуль, тег которого содержит app = myapp
  selector:
    app: myapp
  ports:
  - name: http
 # Порт прослушивания службы  
    port: 80
 # Перенаправить на номер порта серверного модуля   
    targetPort: 80
