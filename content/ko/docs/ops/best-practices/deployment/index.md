---
title: 배포 모범 사례
description: 이스티오 서비스 메시 설정 시 일반적인 모범 사례.
force_inline_toc: true
weight: 10
owner: istio/wg-environments-maintainers
test: n/a
---

We have identified the following general principles to help you get the most
out of your Istio deployments. These best practices aim to limit the impact of
bad configuration changes and make managing your deployments easier.

## Deploy fewer clusters

Deploy Istio across a small number of large clusters, rather than a large number
of small clusters. Instead of adding clusters to your deployment, the best
practice is to use [namespace tenancy](/ko/docs/ops/deployment/deployment-models/#namespace-tenancy)
to manage large clusters. Following this approach, you can deploy Istio across
one or two clusters per zone or region. You can then deploy a control plane on
one cluster per region or zone for added reliability.

## Deploy clusters near your users

Include clusters in your deployment across the globe for **geographic
proximity to end-users**. Proximity helps your deployment have low latency.

## Deploy across multiple availability zones

Include clusters in your deployment **across multiple availability regions
and zones** within each geographic region. This approach limits the size of the
{{< gloss "failure domain" >}}failure domains{{< /gloss >}} of your deployment,
and helps you avoid global failures.