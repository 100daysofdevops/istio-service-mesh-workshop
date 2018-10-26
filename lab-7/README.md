# Lab 7 - Request Routing and Canary Testing

In this lab we are going to get our hands on some of the traffic management capabilities of Istio.

## 7.1 Configure the default route for all services to V1

As part of the bookinfo sample app, there are multiple versions of reviews service. When we load the `/productpage` in the browser multiple times we have seen the reviews service round robin between v1, v2 or v3. As the first exercise, let us first restrict traffic to just V1 of all the services.

Set the default version for all requests to v1 of all service using :

```sh
curl https://raw.githubusercontent.com/leecalcote/istio-service-mesh-workshop/master/deployment_files/istio-1.0.2/virtual-service-all-v1.yaml | istioctl create -f - 
```

This creates a bunch of `virtualservice` and `destinationrule` entries which route calls to v1 of the services.

To view the applied rule:
```sh
istioctl get virtualservice reviews -o yaml
```
Output:
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  creationTimestamp: null
  name: reviews
  namespace: default
  resourceVersion: "6141"
spec:
  hosts:
  - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v1
---
```

Now when we reload the `/productpage` several times, we will ONLY be viewing the data from v1 of all the services, which means we will not see any ratings (any stars).


## 7.2 Content based routing

Let's replace our first rules with a new set. Enable the `ratings` service for test user `jason` by routing `productpage` traffic to `reviews` v2.

```sh
curl https://raw.githubusercontent.com/leecalcote/istio-service-mesh-workshop/master/deployment_files/istio-1.0.2/route-rule-reviews-test-v2.yaml | istioctl replace -f - 
```

To view the applied rule:
```sh
istioctl get virtualservice reviews -o yaml
```
Output:
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  creationTimestamp: null
  name: reviews
  namespace: default
  resourceVersion: "7765"
spec:
  hosts:
  - reviews
  http:
  - match:
    - headers:
        cookie:
          regex: ^(.*?;)?(user=jason)(;.*)?$
    route:
    - destination:
        host: reviews
        subset: v2
  - route:
    - destination:
        host: reviews
        subset: v1
---
```

Now if we login as user `jason` you will be able to see data from reviews v2. While if you NOT logged in or logged in as a different user, you will see data from `reviews` v1.


## 7.3 Canary Testing - Traffic Shifting

### 7.3.1 Reset rules
Before we start the next exercise, lets first reset the routing rules back to our 7.1 rules:

```sh
curl https://raw.githubusercontent.com/leecalcote/istio-service-mesh-workshop/master/deployment_files/istio-1.0.2/route-rule-all-v1.yaml | istioctl replace -f - 
```
Once again, all traffic will be routed to `v1` of all the services. 

### 7.3.2 Canary testing w/50% load
To start canary testing, let's begin by transferring 50% of the traffic from reviews:v1 to reviews:v3 with the following command:

```sh
curl https://raw.githubusercontent.com/leecalcote/istio-service-mesh-workshop/master/deployment_files/istio-1.0.2/route-rule-reviews-50-v3.yaml | istioctl replace -f - 
```

To confirm the rule was applied:
```sh
istioctl get virtualservice reviews -o yaml
```
Output
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
  ...
spec:
  hosts:
  - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v1
      weight: 50
  - route:
    - destination:
        host: reviews
        subset: v3
      weight: 50
```

Now, if we reload the `/productpage` in your browser several times, you should now see red-colored star ratings approximately 50% of the time.


### 7.3.3 Shift 100% to v3
When version v3 of the reviews microservice is considered stable, we can route 100% of the traffic to reviews:v3:

```sh
curl https://raw.githubusercontent.com/leecalcote/istio-service-mesh-workshop/master/deployment_files/istio-1.0.2/route-rule-reviews-v3.yaml | istioctl replace -f - 
```

To confirm the rule was applied:
```sh
istioctl get virtualservice reviews -o yaml
```
Output:
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  creationTimestamp: null
  name: reviews
  namespace: default
  resourceVersion: "9396"
spec:
  hosts:
  - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v3
---
```

Now, if we reload the `/productpage` in your browser several times, you should now see red-colored star ratings 100% of the time.

## [Continue to lab 8 - Fault Injection and Rate Limiting](../lab-8/README.md)
