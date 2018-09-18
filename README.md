# ref-scaler-app
App that monitors the metric app and scales the sample app.

[ref-scaler-impl.yaml](https://github.com/codequester/ref-scaler-app/blob/master/ref-scaler-impl.yaml) - Template file to run the demo.

## Steps
`oc create serviceaccount ref-scaler-impl-sa`

`oc policy add-role-to-user edit -z ref-scaler-impl-sa -n ref-scaler-impl`

`export SA_TOKEN="$(oc sa get-token ref-scaler-impl-sa)"`

`oc process -f ref-scaler-impl.yaml -p NAMESPACE=ref-scaler-impl -p OS_CLUSTER_HOST=master.na39.openshift.opentlc.com -p ROUTE_SUB_DOMAIN=apps.na39.openshift.opentlc.com -p SA_TOKEN=${SA_TOKEN} | oc create -f -`

OS_CLUSTER_HOST and ROUTE_SUB_DOMAIN needs to be changed based on the deployment context.
