# Day 8

## Tekton Pipeline
- has many Tasks
- each Task has many Steps
- each Task is a Pod
- each Step within a Task is a Container within a Pod
- if there are 3 Steps in a Task, Tekton will a Pod with 3 Containers in it

## ⛹️‍♂️ Lab - Writing our first Tekton task
```
cd ~/openshift-sep-2022-batch1
git pull
cd Day8/Tekton/lab1

oc apply -f task1.yml
oc get tasks
tkn task list
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc apply -f task.yml</b>
task.tekton.dev/hello created
(jegan@tektutor.org)$ <b>oc get tasks</b>
NAME    AGE
hello   6s
(jegan@tektutor.org)$ <b>tkn task list</b>
NAME    DESCRIPTION   AGE
hello                 10 seconds ago
</pre>

## ⛹️‍♂️ Lab - Executing your first Tekton task
```
tkn task start hello
tkn taskrun logs hello-run-mdrx6 -f -n jegan
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>tkn task start hello</b>
TaskRun started: hello-run-mdrx6

In order to track the TaskRun progress run:
tkn taskrun logs hello-run-mdrx6 -f -n jegan
(jegan@tektutor.org)$ <b>tkn taskrun logs hello-run-mdrx6 -f -n jegan</b>
[echo] Hello Tekton !
</pre>
