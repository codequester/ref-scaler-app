# ref-scaler-app
App that monitors the metric app and scales the sample app.

[ref-scaler-impl.yaml](https://github.com/codequester/ref-scaler-app/blob/master/ref-scaler-impl.yaml) - Template file to run the demo.

## Steps
`oc create serviceaccount ref-scaler-impl-sa`

`oc policy add-role-to-user edit -z ref-scaler-impl-sa -n ref-scaler-impl`

`export SA_TOKEN="$(oc sa get-token ref-scaler-impl-sa)"`

`oc process -f ref-scaler-impl.yaml -p NAMESPACE=ref-scaler-impl -p OS_CLUSTER_HOST=master.na39.openshift.opentlc.com -p ROUTE_SUB_DOMAIN=apps.na39.openshift.opentlc.com -p SA_TOKEN=${SA_TOKEN} | oc create -f -`

*OS_CLUSTER_HOST* and *ROUTE_SUB_DOMAIN* needs to be changed based on the deployment context.

## Parameter List
```
- description: The number of pods that will be scaled up when the custom metric limit is reached
  displayName: Scale up count
  name: SCALE_UP_COUNT
  required: true
  value: "3"
- description: The number of pods that will be scaled down when the application exhibits normal behavior and if there are more pods than the configure default
  displayName: Scale down count
  name: SCALE_DN_COUNT
  required: true
  value: "2"
- description: Maximum pod count when scaling up.
  displayName: Max pod count
  name: MAX_POD_COUNT
  required: true
  value: "10"
- description: Minimum pod count when scaling down.
  displayName: Min pod count
  name: MIN_POD_COUNT
  required: true
  value: "2"
- description: Maximum queue depth. Pods will be scaled up if queue depth reaches the max values for a set period of time.
  displayName: Max queue depth
  name: MAX_Q_DEPTH
  required: true
  value: "500"
- description: The period of time (in secs) the app will wait to scaling up or down based on the queue depth 
  displayName: Scale time threshold
  name: SCALE_TIME_THRESHOLD_SECS
  required: true
  value: "30"
- description: The routing sub domain as configured by the cluster administrator
  displayName: Routing sub domain
  name: ROUTE_SUB_DOMAIN
  required: true
- description: The openshift cluster host
  displayName: Openshift Cluster Host
  name: OS_CLUSTER_HOST
  required: true
- description: The Auth token for the service account
  displayName: Service account token
  name: SA_TOKEN
  required: true
- description: The name of the Project 
  displayName: Project name
  name: NAMESPACE
  required: true
```
