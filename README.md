# Simple Tecton + RHACS demo

* Fork this and the https://github.com/mglantz/q-app repository.
* Ensure pom.xml in https://github.com/mglantz/q-app says 1.7.3.Final version of Quarkus.
* Point quarkus-pipeline.yaml to your own git repository.
```
git clone https://github.com/mglantz/rhacs-demo
cd rhacs-demo
oc create acstest
oc new-app --name=q-app-git quay.io/quarkus/ubi-quarkus-native-s2i:20.1.0-java11~https://github.com/tqvarnst/q-app.git
oc create -f custom_image_check.json
oc create -f custom_image_scan.json
oc create -f acs_quarkus_policy.json
oc create -f pipeline_pv.json
```
* Create ACS policy from acs_quarkus_policy.json.
* Run pipeline, select PV named actest. See how pipeline run fails.
* Change q-app pom.xml to version 1.9.1.Final of Quarkus.
* Re-run pipeline and see how pipeline succeeds.
