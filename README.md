# How to find the None values in pandas dataframe and get the index and column? 

```python

# we can try this :
import pandas as pd
data = {'name': ['m', 'a', None, 'y'], 'age':[30, 28, 26, None]}
df = pd.DataFrame(data=data)

# this will give df with index, col as index and true or false as values 
null_val = df.isna().stack()

# this will return only the true as (MultiIndex) then we convert it to list so we will have list of tuples at end 
null_val = null_val[null_val].index.tolist()
print(null_val)
# out --> [(2, 'name'), (3, 'age')]
```

# How to run your Project in Rancher Kubernetes Cluster ?

- Install kubectl on your machine
- Create name space in kubernetes: to organize resources and isolate them from any other parts of the cluster, we need to create a name space
```bash
kubectl create namespace <my-namespace>
```
- to confirm that namespace is created:
```bash
kubectl get namespaces
```
- Prepare configuration and create ConfigMap, only when your have secrets, and you save them in .env File (APIs)
- Create the ConfigMap using the following command:
```bash
kubectl create configmap <my-config> --from-env-file=.env -n <my-namespace>
```
- confirm it has been created:
```bash
kubectl get configmaps -n <my-namespace>
```
- Create Dockerfile
```Dockerfile
FROM python:3.8-slim
WORKDIR /app
COPY requirements.txt /app/
RUN .....
CMD ["python", "scripts/myscript.py"]
```
- Build the Docker image
```bash
docker build -t <my-image-name> .
```
verify by :
```bash
docker images
docker image ls -a
```
- push the Docker image to Registry (Gitlab)
```bash
docker push <my-image-name>
```
- Create Kubernetes Job
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: download-data-job
  namespace: <my-namespace>
spec:
  template:
    spec:
      containers:
        - name: download-data
          image: <my-image-name>
          command: ["python3", "scripts/myscript.py"]
          envFrom:
            - configMapRef:
                name: my-config

          volumeMounts:
            - name: shared-volume
              mountPath: /app/data  

      restartPolicy: Never
      imagePullSecrets:
        - name: <username>
      volumes:
        - name: shared-volume
          persistentVolumeClaim:
            claimName: shared-storage
```
- Apply the Job to kubernetes
```bash
kubectl apply -f download-data-job.yaml
```
- to check:
```bash
kubectl get jobs -n <my-namespace>
```
- Monitor jobs
```bash
kubectl logs -f <pod-name> -n <my-namespace>
```
 - to get pod name:
 ```bash
kubectl get pods -n <my-namespace>
```
- Clean up : delete the job
```bash
kubectl delete job download-data-job -n <my-namespace>
```
# What does `du -sh` and `ls -lh` mean?

```bash
du -sh
ls -lh
```
- du stands for disk usage and -s for size and h for human readable , it is used to get the size of folder
- ls stands for lists , -l as a list , h for human readable


