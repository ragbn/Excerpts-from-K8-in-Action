# Observability
## Liveness Probes
* Kubernetes can check if a container is still alive through liveness probes. You can specify a liveness probe for each container in the pod’s specification. Kubernetes will periodically execute the         probe and restart the container if the probe fails.
* Types of liveness probes: An Exec probe, An HTTP GET probe, A TCP Socket probe.
* Creating httpGet livenss probe
![Liveness Probe](https://github.com/ragbn/Excerpts-from-K8-in-Action/blob/master/images/2020-06-11_02-09-39.png?raw=true)
* Beside the liveness probe options you specified explicitly, you can also see additional properties, such as delay, timeout, period, and so on.
* The delay=0s part shows that the probing begins immediately after the container is started. The timeout is set to only 1 second, so the container must return a response in 1 second or the probe is counted as failed. The container is probed every 10 seconds (period=10s) and the container is restarted after the probe fails three consecutive times (#failure=3).
* Liveness probe with an inital delay.
![Liveness Probe](https://github.com/ragbn/Excerpts-from-K8-in-Action/blob/master/images/2020-06-11_02-09-07.png?raw=true)

## Readiness Probes
* The readiness probe is invoked periodically and determines whether the specific pod should receive client requests or not. When a container’s readiness probe returns success, it’s signaling that the container is ready to accept requests.
* When a container is started, Kubernetes can be configured to wait for a configurable amount of time to pass before performing the first readiness check.
* Unlike liveness probes, if a container fails the readiness check, it won’t be killed or restarted. This is an important distinction between liveness and readiness probes.
* Types of readiness probes: An Exec probe, An HTTP GET probe, A TCP Socket probe.
* Creating pod with readiness probe.
![Readiness Probe](https://github.com/ragbn/Excerpts-from-K8-in-Action/blob/master/images/2020-06-11_02-19-35.png?raw=true)
* A readinesProbe may be defined for each container in the pod.

## Logging
* Containerized applications usually log to the standard output and standard error stream instead of writing their logs to files. This is to allow users to view logs of different applications in a simple, standard way.
```bash
$ kubectl logs kubia-manual
```
* If your pod includes multiple containers, you have to explicitly specify the container name by including the -c container name option when running kubectl logs.
```bash
$ kubectl logs kubia-manual -c kubia
Kubia server starting...
```
* When you want to figure out why the previous container terminated, you’ll want to see those logs instead of the current container’s logs.
```bash
$ kubectl logs mypod --previous
```
