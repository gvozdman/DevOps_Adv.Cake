# DevOps_Adv.Cake

Репозиторий создан с целью выполнения технического задания  
для дальнейшего трудоустройства в компанию Adv.Cake  
на вакансию DevOps Engineer

## Дано

Создайте Deployment для приложения на Python в Kubernetes, который использует ConfigMap  
для передачи переменных окружения.

## Решение

Так как flask работает на python за основу бы взят образ `tiangolo/uwsgi-nginx-flask:python3.9-2025-02-24` из  
docker-hub.

По условию задания используется ConfigMap, что передаёт переменную окружения в pod.

```yaml
FLASK_HELLO_MESSAGE: "Welcome to My Flask App!"
```

Внутренний порт pod использует 80 в то время как наружу прокидывает порт 30090.

## Пояснения к файлам

 - flask-configmap.yaml Файл конфигурации ConfigMap, который передает переменные окружения в приложение  
 - flask-deployment.yaml Файл, описывающий деплоймент приложения в Kubernetes
 - service-nodeport.yaml Конфигурация сервиса с типом NodePort, который предоставляет доступ к приложению через внешний порт 30090 на любой ноде кластера


## Выполнения

Выполняем файлы

```shell
kubectl apply -f flask-configmap.yaml
kubectl apply -f flask-deployment.yaml
kubectl apply -f service-nodeport.yaml
```

Вывод

```shell
$ kubectl get svc flask-app-service

NAME                 TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
flask-app-service   NodePort   10.106.108.48   <none>        80:30090/TCP   39m
```

```shell
$ kubectl get pods

NAME                                          READY   STATUS        RESTARTS   AGE
flask-app-deployment-85d64fcb99-5hf6p        1/1     Running       0          26m
flask-app-deployment-85d64fcb99-b62gr        1/1     Running       0          26m
flask-app-deployment-85d64fcb99-b9w5z        1/1     Running       0          26m
```

Смотрим переменные окружения в pod

```shell
$ kubectl exec -it deployment/flask-app-deployment -- env
PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=flask-app-deployment-85d64fcb99-b62gr
LANG=C.UTF-8
GPG_KEY=E3FF2839C048B25C084DEBE9B26995E310250568
PYTHON_VERSION=3.9.21
PYTHON_SHA256=3126f59592c9b0d798584755f2bf7b081fa1ca35ce7a6fea980108d752a05bb1
PYTHONDONTWRITEBYTECODE=1
PYTHONUNBUFFERED=1
UWSGI_INI=/app/uwsgi.ini
UWSGI_CHEAPER=2
UWSGI_PROCESSES=16
NGINX_MAX_UPLOAD=0
NGINX_WORKER_PROCESSES=1
LISTEN_PORT=80
STATIC_URL=/static
STATIC_PATH=/app/static
STATIC_INDEX=0
PYTHONPATH=/app
FLASK_HELLO_MESSAGE=Welcome to My Flask App!
KUBERNETES_SERVICE_HOST=10.96.0.1
NGINX_SERVICE_PORT_80_TCP=tcp://10.97.238.8:80
DJANGO_APP_SERVICE_PORT_80_TCP=tcp://10.106.108.48:80
DJANGO_APP_SERVICE_PORT_80_TCP_PORT=80
DJANGO_APP_SERVICE_PORT_80_TCP_ADDR=10.106.108.48
DJANGO_APP_SERVICE_PORT=tcp://10.106.108.48:80
KUBERNETES_SERVICE_PORT=443
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP_PROTO=tcp
NGINX_SERVICE_PORT=tcp://10.97.238.8:80
NGINX_SERVICE_PORT_80_TCP_PORT=80
DJANGO_APP_SERVICE_SERVICE_PORT=80
DJANGO_APP_SERVICE_PORT_80_TCP_PROTO=tcp
KUBERNETES_PORT=tcp://10.96.0.1:443
NGINX_SERVICE_PORT_80_TCP_PROTO=tcp
DJANGO_APP_SERVICE_SERVICE_HOST=10.106.108.48
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
NGINX_SERVICE_SERVICE_HOST=10.97.238.8
NGINX_SERVICE_SERVICE_PORT=80
NGINX_SERVICE_PORT_80_TCP_ADDR=10.97.238.8
TERM=xterm
HOME=/root
```

## Вывод

Задание выполнено в соответствии с требованиями.  
Прошу рассмотреть мою кандидатуру для дальнейшего трудоустройства.
