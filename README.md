# Festivalstream

Dit project bevat de infrastructuur en applicatieconfiguratie voor het Festivalstream platform. Het platform is ontworpen om te draaien op een Kubernetes cluster en maakt gebruik van een GitOps-workflow via ArgoCD.

## Projectstructuur

De repository is als volgt gestructureerd:

* **`ansible/`**: Ansible playbooks en configuratiebestanden voor het beheren van de nodes (zoals het toevoegen van worker nodes).
* **`apps/`**: Kubernetes manifesten voor de verschillende services (bijvoorbeeld de `streaming-service`).
* **`cluster-config/`**: Kubernetes clusterconfiguraties, geordend in submappen:
    * **`TrueNAS/`**: Configuratie voor persistente opslag via TrueNAS (CSI-driver, PVC's).
    * **`calico/`**: NetworkPolicies voor netwerkscheiding tussen namespaces.
    * **`ingress/`**: Ingress-configuraties (NGINX) voor het routeren van extern verkeer naar services zoals ArgoCD, Chat, Dashboard en Grafana.
    * **`metallb/`**: Configuratie voor de MetalLB load balancer.
    * **`metrics-server/`**: Installatiebestanden voor de Kubernetes Metrics Server.
    * **`monitoring/`**: De monitoring stack met Prometheus, Grafana, Alertmanager en Loki.
    * **`namespaces.yaml`**: Definieert de basis namespaces (`vod`, `chat`, `streaming`, `monitoring`, etc.).

## Vereisten

* Een functioneel Kubernetes cluster.
* Ansible (voor initieel node beheer).
* `kubectl` geconfigureerd voor clusterbeheer.
* Toegang tot deze GitHub repository.
* ArgoCD Installatie met GitHub koppeling.

## Deployment Workflow (GitOps)

De implementatie van de infrastructuur en applicaties verloopt via een geautomatiseerde GitOps-aanpak:

1. **Cluster Voorbereiding:** Het Kubernetes-cluster wordt gebouwd. Worker nodes worden toegevoegd via de scripts in de `ansible/` map.
2. **Automatische Implementatie:** ArgoCD implementeert vervolgens automatisch de manifesten uit de mappen `apps/` en `cluster-config/`. Elke nieuwe commit in de repository wordt door ArgoCD gedetecteerd en direct gesynchroniseerd met het cluster.
