## Setup Development Environment
```
# Setup PATH
export GOPATH=/Volumes/Samsung_T5/gopath
export PATH=/Volumes/Samsung_T5/gopath/bin/:$PATH
cd $GOPATH/src/github.com/sub-mod/amun-operator/


# create operator
export DEPNOLOCK=1
operator-sdk new amun-operator


# here is an example to create a new Controller
operator-sdk add api --api-version=thoth.redhat.com/v1alpha1 --kind=AmunService
operator-sdk generate k8s
operator-sdk build quay.io/sub_mod/amun-operator
docker push quay.io/sub_mod/amun-operator
sed -i "" 's|REPLACE_IMAGE|quay.io/sub_mod/amun-operator|g' deploy/operator.yaml
```

## Install Operator
```
# Ensure you have an user with cluster-admin access
oc login <cluster>
oc whoami
oc adm policy add-cluster-role-to-user cluster-admin <user> --as=system:admin

# create the operator SA, RBAC, CRD as cluster-admin
oc create -f deploy/service_account.yaml
oc create -f deploy/role.yaml
oc create -f deploy/role_binding.yaml
oc create -f deploy/crds/thoth_v1alpha1_amunservice_crd.yaml 
oc create -f deploy/operator.yaml
```

## Delete everything

```
oc delete -f deploy/*.yaml
``` 