


#### Getting Started


Install transmute minikube:

```
curl -Ls https://git.io/transmute_bootstrap | bash
transmute k8s provision-minikube openfaas-cluster
transmute k8s init openfaas-cluster
```

If all goes well, you can curl IPFS through the kong gateway!
use `kubectl get svc` to get the correct port (it won't be 32443) :

```
curl -k https://ipfs.transmute.minikube:32443/api/v0/id
```


#### Install FaaS

```
brew install faas-cli
kubectl apply -f https://raw.githubusercontent.com/openfaas/faas-netes/master/namespaces.yml
helm repo add openfaas https://openfaas.github.io/faas-netes/
helm upgrade --install openfaas openfaas/openfaas --namespace openfaas --set functionNamespace=openfaas-fn --set rbac=false

minikube service list
```

Set the faas cli gateway to the `gateway-external` from above.

```
faas-cli list --gateway http://192.168.99.100:31112
cd funcs

faas-cli deploy --yaml faas-leftpad/stack.yml --gateway http://192.168.99.100:31112
curl --data "austin" http://192.168.99.100:31112/function/leftpad

curl --data "austin" http://192.168.99.100:31112/function/leftpad \
  -H "X-Padding: 80"
```


#### References

- https://github.com/faas-and-furious/faas-leftpad/blob/master/README.md
- https://medium.com/@lizrice/getting-started-with-openfaas-on-minikube-8d51987f5bbb
- https://github.com/openfaas/faas/issues/507