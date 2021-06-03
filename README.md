# Simple Tecton + RHACS demo
```
git clone https://github.com/mglantz/rhacs-demo
cd rhacs-demo
oc create acstest
oc create -f custom_image_check.json
oc create -f custom_image_scan.json
oc create -f acs_quarkus_policy.json
oc create -f pipeline_pv.json
```
* Run pipeline
