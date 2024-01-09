## Virtual services
- set of routing rules
- strongly decoupling where clients send their requests from the destination workloads that actually implement them
- Without virtual services, Envoy distributes traffic using least requests load balancing between all service instances
- specify traffic behavior for one or more hostnames
- Address multiple application services through a single virtual service
- Configure traffic rules in combination with gateways to control ingress and egress traffic.