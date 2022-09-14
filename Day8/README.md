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

oc apply -f task.yml
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

## ⛹️‍♂️ Lab - Creating a Tekton task with multiple steps
```
cd ~/openshift-sep-2022-batch1
git pull
cd Day8/Tekton/lab2

oc apply -f task.yml
tkn task list
tkn task start hello-task-with-multiple-steps
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc apply -f task.yml</b>
task.tekton.dev/hello-task-with-multiple-steps created
(jegan@tektutor.org)$ <b>tkn task list</b>
NAME                             DESCRIPTION   AGE
hello                                          24 minutes ago
hello-task-with-multiple-steps                 6 seconds ago
(jegan@tektutor.org)$ <b>tkn task start hello-task-with-multiple-steps</b>
TaskRun started: hello-task-with-multiple-steps-run-plbzg

In order to track the TaskRun progress run:
tkn taskrun logs hello-task-with-multiple-steps-run-plbzg -f -n jegan
(jegan@tektutor.org)$ <b>tkn taskrun logs hello-task-with-multiple-steps-run-plbzg -f -n jegan</b>
[step-1] Step 1 => Hello TekTon !

[step-2] Step 2 => Hello TekTon !

[step-3] Step 3 => Hello TekTon !
</pre>

## 
