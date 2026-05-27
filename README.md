# Printolito App

Printolito is a WordPress-based web application deployed within the Hub-and-Spoke GitOps architecture.

## The Wrapper Chart Pattern

Unlike standard applications, Printolito represents an off-the-shelf software deployment (Bitnami WordPress) operating under strict corporate network constraints. It uses the **Wrapper Helm Chart** pattern to bypass outbound MITM proxy limitations on the Kubernetes nodes.

*   **Vendored Dependencies:** The upstream WordPress Helm chart is packaged and vendored directly into this repository (`charts/printolito/charts/wordpress-*.tgz`) using `helm dependency update`. 
*   **Offline Capability:** By vendoring the `.tgz` artifact, ArgoCD does not need to reach out to the internet (or Docker Hub/Bitnami charts) to render the manifests, sidestepping proxy blockades.
*   **GitOps Delivery:** ArgoCD deploys this chart to the `printolito-app-prod` spoke cluster, pulling environment-specific credentials and Bitnami image tag overrides (like forcing `latest` tags) from the `gitops-config` repository.
*   **Namespace:** Workloads are deployed to the app-owned `printolito-app` namespace, following the same app namespace convention as the other repositories.

This repository holds the structure needed to package and deliver third-party applications securely and reliably in restricted environments.
