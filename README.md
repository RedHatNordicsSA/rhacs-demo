# Simple Tecton + RHACS demo
```
git clone https://github.com/mglantz/rhacs-demo
cd rhacs-demo
oc create acstest
oc create -f ac
```

* Create cluster tasks: custom-image-check and custom-image-scan, then create the pipeline from custom-pipeline.
* Go into RHACS and configure registry.redhat.io integration and create policy from acs_policy.json.
* Run pipeline and see it work.
