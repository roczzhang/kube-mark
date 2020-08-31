# kube-mark
````yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: kubemark
  name: kubemark
  namespace: kube-mark
spec:
  replicas: 10
  selector:
    matchLabels:
      app: kubemark
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: kubemark
    spec:
      containers:
      - command:
        - /kubemark
        - --morph=kubelet
        - --kubeconfig=/etc/config/config
        - --name=$(POD_NAME)
        env:
        - name: POD_NAME
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.name
        image: mirrors.tencent.com/k8s.io/kubemark:1.0.1
        imagePullPolicy: Always
        name: kubemark
        resources: {}
        securityContext:
          privileged: true
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        volumeMounts:
        - mountPath: /etc/config
          name: config
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
      volumes:
      - configMap:
          defaultMode: 420
          name: config
        name: config


````
