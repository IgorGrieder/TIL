# Load Balancers

Horizontal Scaling strategy for balancing servers operations.

The load balancer doesn't do any real work, it just centralizes and sends the
processing work to other server when it arrives. It only routes the requests
into server/client.

![[Pasted image 20250315172545.png]]

In this strategy the load balancer will always send the processing for a server
that has CPU not being used for any processing.

- link Cloud Fare: <https://www.cloudflare.com/pt-br/learning/performance/types-of-load-balancing-algorithms/>
