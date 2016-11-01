---
title: Balancers
permalink: /guide/balancers/
phase: run
---

A balancer is a stable network endpoint that distributes traffic to the individual containers of a service.

Using a balancer enables you to interact with a service over the network without knowledge of the service's containers or the host they're running on.

A balancer is defined with the `ports:` section of `docker-compose.yml`.

<pre class="file yaml" title="docker-compose.yml">
<span class="diff-u">version: '2'</span>
<span class="diff-u">services:</span>
<span class="diff-u">  web:</span>
<span class="diff-u">    build: .</span>
<span class="diff-u">    command: ["node", "web.js"]</span>
<span class="diff-a">    ports:</span>
<span class="diff-a">     - 80:8000</span>
<span class="diff-u">  worker:</span>
<span class="diff-u">    build: .</span>
<span class="diff-u">    command: ["node", "worker.js"]</span>
<span class="diff-u">    environment:</span>
<span class="diff-u">     - GITHUB_API_TOKEN</span>
</pre>

A published balancer is defined by a pair of external and internal ports in the format `<host port>:<container port>`, e.g. `80:8000`. On the host, the balancer will listen on port `80` and forward requests to app containers on port `8000`.

An _internal_ balancer is defined by a single port, e.g. `8000`. Here, port `8000` is _exposed_, but not _published_. The balancer will not listen for external requests on the host, but will listen on port `8000` on the internal network (i.e. from containers of other services defined in `docker-compose.yml`), and forward requests to fellow containers that are bound to port `8000`.

Run `convox doctor` to validate your balancer definitions:

<pre class="terminal">
<span class="command">convox doctor</span>

### Run: Balancer
[<span class="pass">✓</span>] Application exposes ports
[<span class="pass">✓</span>] Service <span class="service">web</span> has valid ports
</pre>

Now that you have defined Balancers, you can [add a Database](/guide/databases/).
