# Day 9

## ⛹️‍♀️ Lab - Finding the TekTon Controller that monitors TaskRun Resource
```
oc get po -n openshift-pipelines | grep tekton-pipelines-controller
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get po -n openshift-pipelines | grep tekton-pipelines-controller</b>
tekton-pipelines-controller-59dd66fc9f-g5mp8         1/1     Running   0          40h
</pre>

## ⛹️‍♂️ Lab - Watching live logs of Tekton Pipeline Controller when creating a taskrun
From your first terminal window
```
oc logs -f tekton-pipelines-controller-59dd66fc9f-g5mp8 -n openshift-pipelines | grep hello-taskrun
```

From your second terminal window
```
cd ~/openshift-sep-2022-batch1
git pull

cd Day8/lab4
oc apply -f hello-task.yml
oc create -f hello-taskrun-v2.yml
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc logs -f tekton-pipelines-controller-59dd66fc9f-g5mp8 -n openshift-pipelines | grep hello-taskrun</b>
{"level":"info","ts":"2022-09-13T11:56:18.627Z","logger":"tekton-pipelines-controller.event-broadcaster","caller":"record/event.go:285","msg":"Event(v1.ObjectReference{Kind:\"TaskRun\", Namespace:\"jegan\", Name:\"<b>hello-taskrun-w8q2b</b>\", UID:\"d8f5e250-e448-48d2-991e-005faae82abf\", APIVersion:\"tekton.dev/v1beta1\", ResourceVersion:\"19321537\", FieldPath:\"\"}): type: 'Normal' reason: 'Started' ","commit":"4335ca0"}
{"level":"info","ts":"2022-09-13T11:56:18.800Z","logger":"tekton-pipelines-controller","caller":"taskrun/taskrun.go:499","msg":"Successfully reconciled taskrun hello-taskrun-w8q2b/jegan with status: &apis.Condition{Type:\"Succeeded\", Status:\"Unknown\", Severity:\"\", LastTransitionTime:apis.VolatileTime{Inner:time.Date(2022, time.September, 13, 11, 56, 18, 800065672, time.Local)}, Reason:\"Pending\", Message:\"Pending\"}","commit":"4335ca0","knative.dev/controller":"github.com.tektoncd.pipeline.pkg.reconciler.taskrun.Reconciler","knative.dev/kind":"tekton.dev.TaskRun","knative.dev/traceid":"997aae18-8a4a-44c3-bff4-a623ad32aa5b","knative.dev/key":"jegan/hello-taskrun-w8q2b"}
{"level":"info","ts":"2022-09-13T11:56:18.800Z","logger":"tekton-pipelines-controller.event-broadcaster","caller":"record/event.go:285","msg":"Event(v1.ObjectReference{Kind:\"TaskRun\", Namespace:\"jegan\", Name:\"<b>hello-taskrun-w8q2b</b>\", UID:\"d8f5e250-e448-48d2-991e-005faae82abf\", APIVersion:\"tekton.dev/v1beta1\", ResourceVersion:\"19321537\", FieldPath:\"\"}): type: 'Normal' reason: 'Pending' Pending","commit":"4335ca0"}
{"level":"info","ts":"2022-09-13T11:56:18.887Z","logger":"tekton-pipelines-controller","caller":"taskrun/taskrun.go:499","msg":"Successfully reconciled taskrun hello-taskrun-w8q2b/jegan with status: &apis.Condition{Type:\"Succeeded\", Status:\"Unknown\", Severity:\"\", LastTransitionTime:apis.VolatileTime{Inner:time.Date(2022, time.September, 13, 11, 56, 18, 887236098, time.Local)}, Reason:\"Pending\", Message:\"pod status \\\"Initialized\\\":\\\"False\\\"; message: \\\"containers with incomplete status: [prepare place-scripts]\\\"\"}","commit":"4335ca0","knative.dev/controller":"github.com.tektoncd.pipeline.pkg.reconciler.taskrun.Reconciler","knative.dev/kind":"tekton.dev.TaskRun","knative.dev/traceid":"211a7f37-eeda-4ebc-b1f1-86f653a35574","knative.dev/key":"jegan/hello-taskrun-w8q2b"}
</pre>

## ⛹️‍♀️ Lab - Accessing ConfigMap from a Tekton Task
```
cd ~/openshift-sep-2022-batch1
git pull
cd Day9/task-with-configmap

oc apply -f configmap.yml
oc create -f taskrun.yml 
tkn tr logs -f --last
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc create -f task.yml</b> 
configmap/tools-path-cm created
taskrun.tekton.dev/taskrun-with-configmap-dg7ww created
(jegan@tektutor.org)$ <b>tkn tr logs -f --last</b>
[read-tools-path-from-cm] /usr/lib/jdk11 
[read-tools-path-from-cm] 
[read-tools-path-from-cm] /usr/share/maven
</pre>

## ⛹️‍♀️ Lab - Accessing Secret from a Tekton Task
```
cd ~/openshift-sep-2022-batch1
git pull
cd Day9/task-with-secret

oc apply -f secret.yml
oc create -f taskrun.yml 
tkn tr list
tkn tr logs -f --last
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc apply -f secret.yml</b>
secret/openshift-login-credentials created
(jegan@tektutor.org)$ <b>oc create -f taskrun.yml</b>
taskrun.tekton.dev/task-with-secrets-f9lxc created
(jegan@tektutor.org)$ <b>tkn tr list</b>
NAME                           STARTED          DURATION   STATUS
task-with-secrets-f9lxc        6 seconds ago    ---        Running(Pending)
task-with-secrets-v6nvd        8 minutes ago    12s        Succeeded
task-with-secrets-tbfnf        9 minutes ago    2m26s      Succeeded
taskrun-with-configmap-5f6bh   31 minutes ago   11s        Succeeded
taskrun-with-configmap-q7fwp   31 minutes ago   11s        Succeeded
taskrun-with-configmap-mbb8x   33 minutes ago   12s        Succeeded
taskrun-with-configmap-dg7ww   45 minutes ago   11s        Succeeded
taskrun-with-configmap-n7hxp   50 minutes ago   14s        Failed
taskrun-with-configmap-qcf7d   50 minutes ago   12s        Failed
taskrun-with-configmap-nntgl   51 minutes ago   11s        Succeeded
taskrun-with-configmap-52b7w   51 minutes ago   42s        Succeeded
hello-taskrun-v1-8fptl         1 hour ago       11s        Succeeded
(jegan@tektutor.org)$ <b>tkn tr logs -f --last</b>
[c1] kubeadmin-e 
[c1] 
[c1] my-secret-password
</pre>

## ⛹️‍♀️ Lab - Using Persistent Volume and Persistent Volume claim in Tekton task
```
cd ~/openshift-sep-2022-batch1
git pull
cd Day9/task-with-persistentvolume

oc apply -f tekton-pv.yml
oc apply -f tekton-pvc.yml
oc create -f task.yml

tkn tr list
tkn tr logs -f --last
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc apply -f tekton-pv.yml</b>
persistentvolume/tekton-pv-jegan created
(jegan@tektutor.org)$ <b>oc apply -f tekton-pvc.yml</b>
persistentvolumeclaim/tekton-pvc-jegan created
(jegan@tektutor.org)$ <b>ls</b>
README.md  task.yml  tekton-pvc.yml  tekton-pv.yml
(jegan@tektutor.org)$ <b>oc create -f task.yml</b>
taskrun.tekton.dev/taskrun-with-pv-nlw2t created

(jegan@tektutor.org)$ <b>tkn tr list</b>
NAME                           STARTED         DURATION   STATUS
taskrun-with-pv-nlw2t          4 seconds ago   ---        Running(Pending)
task-with-secrets-f9lxc        1 hour ago      10s        Succeeded
task-with-secrets-v6nvd        1 hour ago      12s        Succeeded
task-with-secrets-tbfnf        1 hour ago      2m26s      Succeeded
taskrun-with-configmap-5f6bh   1 hour ago      11s        Succeeded
taskrun-with-configmap-q7fwp   1 hour ago      11s        Succeeded
taskrun-with-configmap-mbb8x   1 hour ago      12s        Succeeded
taskrun-with-configmap-dg7ww   1 hour ago      11s        Succeeded
taskrun-with-configmap-nntgl   1 hour ago      11s        Succeeded
taskrun-with-configmap-52b7w   1 hour ago      42s        Succeeded
hello-taskrun-v1-8fptl         2 hours ago     11s        Succeeded
(jegan@tektutor.org)$ <b>tkn tr logs -f --last</b>

[print-path] /my/myworkspace
</pre>

## ⛹️‍♀️ Lab - Task to clone github repo
We need to first install git-clone task from Tekton Hub
```
tkn hub install task git-clone
```

You can create the taskrun now as shown below
```
cd ~/openshift-sep-2022-batch1
git pull
cd Day9/git-clone-task

oc apply -f tekton-pv.yml
oc apply -f tekton-pvc.yml
oc create -f task.yml
tkn tr list
tkn tr logs -f --last
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>ls</b>
task.yml  tekton-pvc.yml  tekton-pv.yml
(jegan@tektutor.org)$ <b>oc get pv,pvc</b>
NAME                               CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                                           STORAGECLASS   REASON   AGE
persistentvolume/ansible-pv        8Gi        RWO            Retain           Bound    ansible-automation-platform/postgres-13-ansible-postgres-13-0                           33d
persistentvolume/tekton-pv-jegan   5Mi        RWX            Retain           Bound    jegan/tekton-pvc-jegan                                                                  34m

NAME                                     STATUS   VOLUME            CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/tekton-pvc-jegan   Bound    tekton-pv-jegan   5Mi        RWX                           34m
(jegan@tektutor.org)$ <b>ls</b>
task.yml  tekton-pvc.yml  tekton-pv.yml
(jegan@tektutor.org)$ <b>oc create -f task.yml</b>
taskrun.tekton.dev/clone-tektutor-github-repo-wvjpz created
(jegan@tektutor.org)$ <b>tkn tr list</b>
NAME                               STARTED          DURATION   STATUS
clone-tektutor-github-repo-wvjpz   7 seconds ago    0s         Failed(TaskRunResolutionFailed)
taskrun-with-pv-nlw2t              34 minutes ago   14s        Succeeded
task-with-secrets-f9lxc            1 hour ago       10s        Succeeded
task-with-secrets-v6nvd            1 hour ago       12s        Succeeded
task-with-secrets-tbfnf            1 hour ago       2m26s      Succeeded
taskrun-with-configmap-5f6bh       2 hours ago      11s        Succeeded
taskrun-with-configmap-q7fwp       2 hours ago      11s        Succeeded
taskrun-with-configmap-mbb8x       2 hours ago      12s        Succeeded
taskrun-with-configmap-dg7ww       2 hours ago      11s        Succeeded
taskrun-with-configmap-n7hxp       2 hours ago      14s        Failed
taskrun-with-configmap-qcf7d       2 hours ago      12s        Failed
taskrun-with-configmap-nntgl       2 hours ago      11s        Succeeded
taskrun-with-configmap-52b7w       2 hours ago      42s        Succeeded
hello-taskrun-v1-8fptl             2 hours ago      11s        Succeeded
(jegan@tektutor.org)$ <b>tkn tr logs -f --last</b>
task git-clone has failed: error when listing tasks for taskRun clone-tektutor-github-repo-wvjpz: tasks.tekton.dev "git-clone" not found
Error: pod for taskrun clone-tektutor-github-repo-wvjpz not available yet
(jegan@tektutor.org)$ <b>tkn hub install task git-clone</b>
Task git-clone(0.8) installed in jegan namespace
(jegan@tektutor.org)$ <b>tkn t list</b>
NAME        DESCRIPTION              AGE
git-clone   These Tasks are Git...   13 seconds ago
(jegan@tektutor.org)$ <b>oc create -f task.yml</b>
taskrun.tekton.dev/clone-tektutor-github-repo-f89d4 created
(jegan@tektutor.org)$ <b>tkn tr list</b>
^[[ANAME                               STARTED          DURATION   STATUS
clone-tektutor-github-repo-f89d4   5 seconds ago    ---        Running(Pending)
clone-tektutor-github-repo-wvjpz   1 minute ago     0s         Failed(TaskRunResolutionFailed)
taskrun-with-pv-nlw2t              36 minutes ago   14s        Succeeded
task-with-secrets-f9lxc            1 hour ago       10s        Succeeded
task-with-secrets-v6nvd            1 hour ago       12s        Succeeded
task-with-secrets-tbfnf            1 hour ago       2m26s      Succeeded
taskrun-with-configmap-5f6bh       2 hours ago      11s        Succeeded
taskrun-with-configmap-q7fwp       2 hours ago      11s        Succeeded
taskrun-with-configmap-mbb8x       2 hours ago      12s        Succeeded
taskrun-with-configmap-dg7ww       2 hours ago      11s        Succeeded
taskrun-with-configmap-n7hxp       2 hours ago      14s        Failed
taskrun-with-configmap-qcf7d       2 hours ago      12s        Failed
taskrun-with-configmap-nntgl       2 hours ago      11s        Succeeded
taskrun-with-configmap-52b7w       2 hours ago      42s        Succeeded
hello-taskrun-v1-8fptl             2 hours ago      11s        Succeeded
(jegan@tektutor.org)$ <b>tkn tr logs -f --last</b>
[clone] + '[' false '=' true ]
[clone] + '[' false '=' true ]
[clone] + '[' false '=' true ]
[clone] + CHECKOUT_DIR=/workspace/output/
[clone] + '[' true '=' true ]
[clone] + cleandir
[clone] + '[' -d /workspace/output/ ]
[clone] + rm -rf '/workspace/output//*'
[clone] + rm -rf '/workspace/output//.[!.]*'
[clone] + rm -rf '/workspace/output//..?*'
[clone] + test -z 
[clone] + test -z 
[clone] + test -z 
[clone] + /ko-app/git-init '-url=https://github.com/tektutor/spring-ms.git' '-revision=master' '-refspec=' '-path=/workspace/output/' '-sslVerify=true' '-submodules=true' '-depth=1' '-sparseCheckoutDirectories='
[clone] {"level":"info","ts":1663158017.738226,"caller":"git/git.go:170","msg":"Successfully cloned https://github.com/tektutor/spring-ms.git @ dd9d75b76029ced3481833315d937cfc6cf3975a (grafted, HEAD, origin/master) in path /workspace/output/"}
[clone] {"level":"info","ts":1663158017.8133032,"caller":"git/git.go:208","msg":"Successfully initialized and updated submodules in path /workspace/output/"}
[clone] + cd /workspace/output/
[clone] + git rev-parse HEAD
[clone] + RESULT_SHA=dd9d75b76029ced3481833315d937cfc6cf3975a
[clone] + EXIT_CODE=0
[clone] + '[' 0 '!=' 0 ]
[clone] + printf '%s' dd9d75b76029ced3481833315d937cfc6cf3975a
[clone] + printf '%s' https://github.com/tektutor/spring-ms.git
</pre>

## ⛹️‍♀️ Lab - Clone GitHub Repository from a Tekton Task
```
cd ~/openshift-sep-2022-batch1
git pull

cd Day9/clone-git-repo
oc apply -f tekton-pv.yml
oc apply -f tekton-pvc.yml
oc create -f task.yml
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc create -f task.yml</b>
taskrun.tekton.dev/clone-tektutor-github-repo-r28fl created
(jegan@tektutor.org)$ <b>tkn taskrun logs -f --last</b>
[clone] + '[' false '=' true ]
[clone] + '[' false '=' true ]
[clone] + '[' false '=' true ]
[clone] + CHECKOUT_DIR=/workspace/output/
[clone] + '[' true '=' true ]
[clone] + cleandir
[clone] + '[' -d /workspace/output/ ]
[clone] + rm -rf '/workspace/output//*'
[clone] + rm -rf '/workspace/output//.[!.]*'
[clone] + rm -rf '/workspace/output//..?*'
[clone] + test -z 
[clone] + test -z 
[clone] + test -z 
[clone] + /ko-app/git-init '-url=https://github.com/tektutor/spring-ms.git' '-revision=master' '-refspec=' '-path=/workspace/output/' '-sslVerify=true' '-submodules=true' '-depth=1' '-sparseCheckoutDirectories='
[clone] {"level":"info","ts":1663225819.4560235,"caller":"git/git.go:170","msg":"Successfully cloned https://github.com/tektutor/spring-ms.git @ dd9d75b76029ced3481833315d937cfc6cf3975a (grafted, HEAD, origin/master) in path /workspace/output/"}
[clone] {"level":"info","ts":1663225819.5435658,"caller":"git/git.go:208","msg":"Successfully initialized and updated submodules in path /workspace/output/"}
[clone] + cd /workspace/output/
[clone] + git rev-parse HEAD
[clone] + RESULT_SHA=dd9d75b76029ced3481833315d937cfc6cf3975a
[clone] + EXIT_CODE=0
[clone] + '[' 0 '!=' 0 ]
[clone] + printf '%s' dd9d75b76029ced3481833315d937cfc6cf3975a
[clone] + printf '%s' https://github.com/tektutor/spring-ms.git
</pre>
