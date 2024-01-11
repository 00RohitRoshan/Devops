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

### Istio Security (Authentication & Authorization)
- To defend against man-in-the-middle attacks, they need `traffic encryption`.
- To provide flexible service access control, they need `mutual TLS` and fine-grained `access policies`.
- To determine who did what at what time, they need `auditing` tools.     

![Achitecture](https://istio.io/latest/docs/concepts/security/overview.svg)

#### Security components
- A `Certificate Authority (CA)` for key and certificate management
- The `configuration API server` distributes to the proxies:
    - authentication policies
    - authorization policies
    - secure naming information
- `Sidecar and perimeter proxies` work as Policy Enforcement Points (PEPs) to secure communication between clients and servers.
- A set of `Envoy proxy extensions` to manage telemetry and auditing

![Components](https://istio.io/latest/docs/concepts/security/arch-sec.svg)

#### Istio Identity
- Workload-to-Workload -  two parties must exchange credentials with their `identity information` for mutual authentication purposes.
- Client - server’s identity is checked against the `secure naming` 
- Server - the server can determine what information the client can access based on the `authorization policies`

|Service Provider | Identity |  
|--|--|  
| Kubernetes: | Kubernetes service account |
|GCE:| GCP service account|
|On-premises (non-Kubernetes):| user account, custom service account, service name, Istio service account, or GCP service account. The custom service account refers to the existing service account just like the identities that the customer’s Identity Directory manages.|

#### Identity and certificate management
- X.509 certificates
[flow](https://istio.io/latest/docs/concepts/security/#pki)
![Identity provissioning flow](https://istio.io/latest/docs/concepts/security/id-prov.svg)

#### Authentication
- 2 Types
    - Peer Authentication (Service-to-Service)
    - Request Authentication (end user)(JWT-JsonWebToken)
- Mutual TLS authentication (peer-to-peer/service-to-service)
    - Permissive mode (help you understand how a policy change can affect your security posture before it is enforced.)
        -  both `plaintext` traffic and `mutual TLS` traffic at the same time. 
    - Secure naming
- [Authentication architecture](https://istio.io/latest/docs/concepts/security/authn.svg)
- Authentication policies
    - Policy storage
    - Selector field
    - Peer authentication
        - Permisive
        - Strict
        - Disable
    - Request authentication
        - Request authentication policies specify the values needed to validate a JSON Web Token (JWT).
    - Principals
- Updating authentication policies

#### Authorization
- Authorization architecture
- Implicit enablement
- Authorization policies
    - Policy Target
    - Value matching
    - Exclusion matching
    - allow-nothing, deny-all and allow-all policy
    - Custom conditions
    - Authenticated and unauthenticated identity
- Using Istio authorization on plain TCP protocols
- Dependency on mutual TLS