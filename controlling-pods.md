#

## Labeling pods

**Check which nodes are labeled with env label**
````
oc get nodes -L env
NAME       STATUS   ROLES           AGE     VERSION           ENV
master01   Ready    master,worker   15d10h   v1.18.3+012b3ec
master02   Ready    master,worker   15d10h   v1.18.3+012b3ec
master03   Ready    master,worker   15d10h   v1.18.3+012b3ec
master03   Ready    master,worker   15d10h   v1.18.3+012b3ec
````

**Set env=dev and env=prod labels and env=ppr**

````
oc label node master01 env=dev
````

````
oc label node master02 env=ppr
````

````
oc label node master03 env=prod
````

**Run all pods of application on the node labeled env=dev**

edit deployment or deployment-config resource:

````
oc edit deployment/my-app

...
Kind: Deployment
...
spec:
...
  template:
  ...
    spec:
    ...
      nodeSelector:
        env: dev
        ....
````

