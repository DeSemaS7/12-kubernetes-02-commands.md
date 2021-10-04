# Задание 1: Запуск пода из образа в деплойменте
В текущем контексте сделал деплоймент и 2 реплики, проверил наличие подов.
![deployment](deployments.png)

# Задание 2: Просмотр логов для разработки
сделал отдельного сервис-юзера web-user и неймспейс web-namespace. 
<br>
Ему сделал роль logs с правами на просмотр логов:
```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: logs
  namespace: web-namespace
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch", "logs", "describe", "get"]
```
<br>

После этого сделал маппинг роли и юзера:
```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: web-user-logs
  namespace: web-namespace
subjects:
- kind: ServiceAccount
  name: web-user
  namespace: web-namespace
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: logs
```
Далее посмотерл токен юзера и вписал всё это дело в конфиг:
![config](config.png)

В результате получил работающее ограничение:
![restrict](restrict.png)

# Задание 3: Изменение количества реплик
Отмасштабировал деплоймент и убедился что все поды ожили:
![scaling](scale.png)


