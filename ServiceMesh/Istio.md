### Envoy Configuration (Traffic management)
[Traffic Management Problems](https://istio.io/latest/docs/ops/common-problems/network-issues/#virtual-service-with-fault-injection-and-retrytimeout-policies-not-working-as-expected)
#### Virtual services
- **set of routing rules ,to a given destination**
- strongly decoupling where clients send their requests from the destination workloads that actually implement them
- Without virtual services, Envoy distributes traffic using least requests load balancing between all service instances
- specify traffic behavior for one or more hostnames
- Address multiple application services through a single virtual service
- Configure traffic rules in combination with gateways to control ingress and egress traffic.

#### Destination Rules
- **What happens to the traffic for that  destination**
- Destination rules are applied after virtual service routing rules are evaluated, so they apply to the traffic’s “real” destination.

#### Load Balancing Options
- Default - least requests
    - Random: Requests are forwarded at random to instances in the pool.
    - Weighted: Requests are forwarded to instances in the pool according to a specific percentage.
    - Round robin: Requests are forwarded to each instance in sequence.

#### Gateways
- Letting to specify which traffic you want to enter or leave the mesh.
- Gateway configurations are applied to **standalone Envoy proxies that are running at the edge of the mesh**, rather than sidecar Envoy proxies running alongside your service workloads.

#### Service Entries
- [references](https://istio.io/latest/docs/reference/config/networking/service-entry/)
- To add an entry to the service registry that Istio maintains internally.
    - Manage traffic for services running outside of the mesh.
    - Redirect and forward traffic for external destinations, such as APIs consumed from the web, or traffic to services in legacy infrastructure.
    - Define retry, timeout, and fault injection policies for external destinations.
    - Run a mesh service in a Virtual Machine (VM) by adding VMs to your mesh.
- Don’t need to add a service entry for every external service
- However, you can’t use Istio features to control the traffic to destinations that aren’t registered in the mesh.

#### Sidecars
[examples](https://istio.io/latest/docs/reference/config/networking/sidecar/)
- Sidecar describes the configuration of the sidecar proxy.
- Fine-tune the set of ports and protocols that an Envoy proxy accepts.
- Limit the set of services that the Envoy proxy can reach.

#### [Network resilience and testing](https://istio.io/latest/docs/concepts/traffic-management/#network-resilience-and-testing)
- Timeouts
- Retries
- [Circuit breakers](https://istio.io/latest/docs/tasks/traffic-management/circuit-breaking/)
- [Fault injection](https://istio.io/latest/docs/tasks/traffic-management/fault-injection/)
