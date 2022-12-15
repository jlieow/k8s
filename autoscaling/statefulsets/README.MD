# 1. Deploy statefulset and service. The service should expose the statefulset and provide a http endpoint.

kubectl apply -f statefulset.yaml

# 2. Deploy horizontalpodautoscaler. The autoscaler should read the CPU metrics of the statefulset.

kubectl apply -f hpa.yaml

# 3. Update the correct http endpoint exposed by the service and generate some load for the stateful set

kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://{ENDPOINT}; done"

# 4. Watch the HPA as the load increases

kubectl get hpa web --watch