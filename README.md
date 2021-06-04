# Simple Tecton + RHACS demo
This is a simple demonstration of building a Quarkus application using Tecton in OpenShift 4.6 and integrating the pipeline to Red Hat Advanced Cluster Security (ACS/RHACS). The demo detect a vulnerability (CVE-2020-25638: hibernate-core: SQL injection) in Quarkus 1.7.3.Final, but updating our build to Quarkus 1.11.6.Final, we can see how our build now passes scanning and can by deployed.

![acs/tecton demo](img/demo.png)

* Provision OCP4 ACS cluster from RHPDS
* Run below commands to setup the demo
```
git clone https://github.com/RedHatNordicsSA/rhacs-demo
cd rhacs-demo
oc create namespace acstest
oc new-app --name=q-app-git quay.io/quarkus/ubi-quarkus-native-s2i:20.1.0-java11~https://github.com/tqvarnst/q-app.git
oc create -f custom_image_check.yaml
oc create -f custom_image_scan.yaml
oc create -f pipeline_pv.yaml
oc get secrets roxsecrets -n stackrox-pipeline-demo -o yaml|grep -v resourceVersion|sed 's/stackrox-pipeline-demo/acstest/g' >roxsecrets.yaml
oc create -f roxsecrets.yaml
```

* Create ACS policy from acs_quarkus_policy.json which will detect an issue in the built image via the RHACS web console.
![acs policy](img/acs.png)

* Run a pipeline that fails. By setting GIT_REVISION to `release` and IMAGE to q-app-git:release, we will build our Quarkus app based Quarkus 1.7.3.Final. CVE-2020-25638 is not fixed in this image, which will show in the scan of the built image. 
![failing pipeline](img/fail.png)

* Run a pipeline that passes scanning. By changing GIT_REVISION to `main` and IMAGE to q-app-git:main, we will build our Quarkus app based on Quarkus 1.11.6.Final which contains the fix for CVE-2020-25638, causing the scan to pass and the app to deploy.
![passing pipeline](img/pass.png)

