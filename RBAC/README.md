# k8s-rbac-demo

```bash
openssl genrsa -out jerry.key 2048
```

- openssl: This is the command-line tool for using the OpenSSL library.
- genrsa: This command generates an RSA private key.
- -out jerry.key: Specifies the file where the generated private key will be saved. In this case, it will be saved to a file named jerry.key.
- 2048: This specifies the size (in bits) of the RSA key to be generated. A 2048-bit key is commonly used and provides good security.


```bash
openssl req -new -key jerry.key -out jerry.csr -subj "/CN=jerry"
```

- openssl: The command-line tool for using the OpenSSL library.
- req: This command is used for creating and processing certificate requests.
- -new: This option indicates that a new CSR is being created.
- -key jerry.key: Specifies the private key to be used to generate the CSR. In this case, it uses the jerry.key file created in the previous step.
- -out jerry.csr: Specifies the file where the generated CSR will be saved. In this case, it will be saved to a file named jerry.csr.
- -subj "/CN=jerry": This specifies the subject of the CSR directly on the command line:

    - /CN=jerry: CN stands for Common Name, which typically represents the domain name or the name of the entity requesting the certificate. Here, it's set to jerry.



```bash
cat jerry.csr | base64 | tr -d '\n'
```

- cat jerry.csr: This part reads and outputs the contents of the jerry.csr file.
- |: This is a pipe, used to pass the output of one command as input to the next command.
- base64: This command encodes the input (in this case, the contents of jerry.csr) into base64 format.
- |: Another pipe to pass the output to the next command.
- tr -d '\n': This command removes all newline characters from the input.


```bash
kubectl apply -f csr.yml
kubectl certificate approve jerry
```

- kubectl: The Kubernetes command-line tool used to interact with the Kubernetes cluster.
- certificate: A Kubernetes resource type for managing certificate requests.
- approve: This subcommand is used to approve a pending certificate signing request (CSR).
- jerry: The name of the CSR that you want to approve.

```bash
kubectl get csr jerry -o jsonpath='{.status.certificate}' | base64 --decode > jerry.crt
```

- kubectl get csr jerry -o jsonpath='{.status.certificate}':

    kubectl get csr jerry: Fetches the CSR named jerry.
    -o jsonpath='{.status.certificate}': Outputs the status of the CSR in a JSONPath format, specifically extracting the certificate field from the status.

- |: This pipe operator passes the output of the previous command to the next command.

- base64 --decode:

    base64: Command-line tool to encode/decode data in base64 format.
    --decode: Decodes the base64-encoded certificate.

- > jerry.crt:

    >: Redirects the output to a file.
    jerry.crt: The name of the file where the decoded certificate will be saved.
    
# Setup Kubeconfig for new user

```bash
kubectl config set-credentials jerry --client-certificate=jerry.crt --client-key=jerry.key
kubectl config get-contexts
kubectl config set-context jerry-context --cluster=kubernetes --namespace=default --user=jerry
kubectl config use-context jerry-context
```

### Checking Permissions for User jerry

```bash
kubectl auth can-i get pods --as=jerry --namespace=default
kubectl auth can-i watch pods --as=jerry --namespace=default
kubectl auth can-i list pods --as=jerry --namespace=default
```

