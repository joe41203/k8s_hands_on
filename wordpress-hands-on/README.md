minikube を起動

```
$ minikube start
```

password を暗号化

```
$ kubectl create secret generic mysql-pass --from-file=password.txt
```

pod と service を作成

```
$ kubectl apply -f mysql-pod.yml
$ kubectl apply -f mysql-service.yml
$ kubectl apply -f wordpress-pod.yml
$ kubectl apply -f wordpress-service.yml
```

dashboard で確認

```
$ minikube dashboard
```

ブラウザで確認

```
$ minikube service wordpress
```

参考

https://github.com/takaishi/hello2018/tree/master/k8s_hands_on
