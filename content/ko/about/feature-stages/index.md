---
title: Feature Status
description: List of features and their release stages.
weight: 10
icon: feature-status
---

<!--
Note: this contains feature status from
https://docs.google.com/spreadsheets/d/1Nbjat-juyQ8AWhkq3njLckmHM8TRL4O-sjm9Bfr9zrU/edit#gid=0
-->

This page lists the relative maturity and support
level of every Istio feature. Please note that the phases (Alpha, Beta, and Stable) are applied to individual features
within the project, not to the project as a whole. Here is a high level description of what these labels mean.

## Feature phase definitions

|            | Alpha      | Beta         | Stable
|-------------------|-------------------|-------------------|-------------------
|   **Purpose**         | Demo-able, works end-to-end but has limitations.  If you use it in production and encounter a serious issue we may not be able to fix it for you, so be sure that you can continue to function if you have to disable it | Usable in production, not a toy anymore | Dependable, production hardened
|   **API**         | No guarantees on backward compatibility    | APIs are versioned         | Dependable, production-worthy. APIs are versioned, with automated version conversion for backward compatibility
|  **Performance**         | Not quantified or guaranteed     | Not quantified or guaranteed         | Performance (latency/scale) is quantified, documented, with guarantees against regression
|   **Deprecation Policy**        | None     | Weak - 3 months         | Dependable,  Firm. 1 year notice will be provided before changes
| **Security** | Security vulnerabilities will be handled publicly as simple bug fixes | Security vulnerabilities will be handled according to our [security vulnerability policy](/about/security-vulnerabilities/) | Security vulnerabilities will be handled according to our [security vulnerability policy](/about/security-vulnerabilities/)

## Istio features

Below is our list of existing features and their current phases. This information will be updated after every monthly release.

### Traffic management

| Feature           | Phase
|-------------------|-------------------
| Protocols: HTTP1.1 / HTTP2 / gRPC / TCP | Stable
| Protocols: Websockets / MongoDB  | Stable
| Traffic Control: label/content based routing, traffic shifting | Stable
| Resilience features: timeouts, retries, connection pools, outlier detection | Stable
| Gateway: Ingress, Egress for all protocols | Stable
| TLS termination and SNI Support in Gateways | Stable
| SNI (multiple certs) at ingress | Stable
| [Locality load balancing](/ko/docs/ops/configuration/traffic-management/locality-load-balancing/) | Beta
| Enabling custom filters in Envoy | Alpha
| CNI container interface | Alpha
| [Sidecar API](/ko/docs/reference/config/networking/sidecar/) | Beta

### Observability

| Feature           | Phase
|-------------------|-------------------
| [Prometheus Integration](/ko/docs/tasks/observability/metrics/querying-metrics/) | Stable
| [Client and Server Telemetry Reporting](https://istio.io/v1.6/ko/docs/reference/config/policy-and-telemetry/) | Stable
| [Service Dashboard in Grafana](/ko/docs/tasks/observability/metrics/using-istio-dashboard/) | Stable
| [Distributed Tracing](/ko/docs/tasks/observability/distributed-tracing/) | Stable
| [Stackdriver Integration - BETTER LINK NEEDED](https://istio.io/v1.6/ko/docs/reference/config/policy-and-telemetry/adapters/) | Stable
| [Distributed Tracing to Zipkin / Jaeger](/ko/docs/tasks/observability/distributed-tracing/) | Beta
| [Trace Sampling](/ko/docs/tasks/observability/distributed-tracing/configurability/#trace-sampling) | Beta

### Security and policy enforcement

| Feature           | Phase
|-------------------|-------------------
| [Service-to-service mutual TLS](/ko/docs/concepts/security/#mutual-tls-authentication)         | Stable
| [Kubernetes: Service Credential Distribution](/ko/docs/concepts/security/#pki)   | Stable
| [Certificate management on Ingress Gateway](/ko/docs/tasks/traffic-management/ingress/secure-ingress) | Stable
| [Pluggable Key/Cert Support for Istio CA](/ko/docs/tasks/security/cert-management/plugin-ca-cert/)        | Stable
| [Authorization](/ko/docs/concepts/security/#authorization)   | Beta
| [End User (JWT) Authentication](/ko/docs/concepts/security/#authentication)  | Beta
| [Automatic mutual TLS](/ko/docs/tasks/security/authentication/authn-policy/#auto-mutual-tls) | Beta
| [VM: Service Credential Distribution](/ko/docs/concepts/security/#pki)         | Beta
| [Mutual TLS Migration](/ko/docs/tasks/security/authentication/mtls-migration)    | Beta

### Core

| Feature           | Phase
|-------------------|-------------------
| [Standalone Operator](/ko/docs/setup/install/operator/) | Beta
| [Kubernetes: Envoy Installation and Traffic Interception](/ko/docs/setup/) | Stable
| [Kubernetes: Istio Control Plane Installation](/ko/docs/setup/) | Stable
| [Attribute Expression Language](https://istio.io/v1.6/ko/docs/reference/config/policy-and-telemetry/expression-language/) | Stable
| Mixer Out-of-Process Adapter Authoring Model | Beta
| [Multicluster Mesh over VPN](/ko/docs/setup/install/multicluster/) | Alpha
| [Kubernetes: Istio Control Plane Upgrade](/ko/docs/setup/) | Beta
| Consul Integration | Alpha
| Basic Configuration Resource Validation | Beta
| Configuration Processing with Galley | Beta
| [Custom Mixer Build Model](https://github.com/istio/istio/wiki/Mixer-Compiled-In-Adapter-Dev-Guide) | deprecated
| [Out of Process Mixer Adapters (gRPC Adapters)](https://github.com/istio/istio/wiki/Mixer-Out-Of-Process-Adapter-Dev-Guide) | Beta
| [Istio CNI plugin](/ko/docs/setup/additional-setup/cni/) | Alpha
| IPv6 support for Kubernetes | Alpha. Dual-stack IPv4 and IPv6 is not supported.
| [Distroless base images for Istio](/ko/docs/ops/configuration/security/harden-docker-images/) | Alpha
| [Virtual Machine Integration](/ko/docs/setup/install/virtual-machine/) | Alpha

{{< idea >}}
Please get in touch by joining our [community](/about/community/) if there are features you'd like to see in our future releases!
{{< /idea >}}
