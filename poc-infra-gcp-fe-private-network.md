# Hybrid GCP & FE (OBS Flexible Engine) lattice on private network

## concepts to be proofed

- techno hands on

## use-case

Deploy pet clinic app on a private network with nodes deployed on Flexible Engine and GCP tenants bridged by a VPN

## difficulties & interrogations

### multiple HttpServers capability-providers

problem

Brook's answer

> So this has to do mainly with how we determine the target of an invocation. The provider ID isn't a unique identifier for a single instance of a provider, it's a unique identifier for the provider itself. So when you have your petclinic API actor, you link that actor to (for example) the open source HTTP server provider ID with a link name of default and a contract of wasmcloud:httpserver
>
> The net effect of this is that if you have two httpserver providers running on two different hosts, if either of those providers receive an HTTP request it will still go to the clinic API actor, this is intentional

