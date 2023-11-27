# RBAC
Ananad - Bala - chandra are the Examppe users i have taken .

Goal: Implement Role-Based Access Control (RBAC) to restrict access to Kubernetes resources based on user roles.

Prerequisites:

A Kubernetes cluster running kops 1.25
Customer CA certificates copied from the master node to the management server
Step 1: Create Users and Generate Keys

Create two users: anand for the development namespace and bala for the production namespace.

Generate RSA public and private key pairs for each user:

Bash
openssl genrsa -out anand.key 2048
openssl req -new -key anand.key -out anand.csr -subj “/CN=anand/O=development”

openssl genrsa -out bala.key 2048
openssl req -new -key bala.key -out bala.csr -subj “/CN=bala/O=production”

Sign the certificates with the CA certificate:
Bash
openssl x509 -req -in anand.csr -CA CA.crt -CAkey CA.key -CAcreateserial -out anand.crt -days 365

openssl x509 -req -in bala.csr -CA CA.crt -CAkey CA.key -CAcreateserial -out bala.crt -days 365

Step 2: Create Configuration Files

Copy the kube.config file from ~/.kube/config to your management server for reference.

Create two configuration files, ANAND-CONFIG and BALA-CONFIG, for anand and bala, respectively.

Modify the configuration files to include the appropriate user certificates and server addresses.

Step 3: Create Roles

Create a YAML file named role.yaml for both anand and bala containing the role definitions.

Deploy the role definitions to Kubernetes using kubectl apply -f role.yaml.

Step 4: Create Role Bindings

Create a YAML file named rolebinding.yaml to bind the roles to the respective users.

Deploy the role bindings to Kubernetes using kubectl apply -f rolebinding.yaml.

Step 5: Create Cluster Role and Cluster Role Binding

Create a YAML file named clusterrole.yaml containing the cluster role definition.

Deploy the cluster role definition to Kubernetes using kubectl apply -f clusterrole.yaml.

Create a YAML file named clusterrolebinding.yaml to bind the cluster role to the chandra user.

Deploy the cluster role binding to Kubernetes using kubectl apply -f clusterrolebinding.yaml.

Step 6: Merge Configuration Files
Step 7: Install kubectx

Download and install kubectx to easily switch between different configuration contexts.

Use kubectx to switch between anand, bala, and chandra contexts.

Conclusion:

By following these steps, you have successfully implemented RBAC in your Kubernetes cluster, restricting access to resources based on user roles and namespaces.
Merge the three configuration files into a single file named mixed-config.txt.

Set the KUBECONFIG environment variable to the merged configuration file:

Bash
export KUBECONFIG=/root/mixed-config.txt

