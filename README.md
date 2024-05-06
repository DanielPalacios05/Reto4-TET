# Implementación de WordPress de Alta Disponibilidad en Kubernetes

Este repositorio contiene un conjunto de archivos de configuración YAML para desplegar un sitio de WordPress de alta disponibilidad en un clúster de Kubernetes. La configuración incluye la gestión de certificados TLS, balanceo de carga y almacenamiento persistente.

## Componentes

- *Certificate.yaml*: Define un recurso Certificate para la encriptación TLS utilizando Let's Encrypt.
- *ClusterIssuer.yaml*: Configura un ClusterIssuer para el servidor ACME para emitir certificados.
- *Load-Balancing-Ingress.yaml*: Configura un recurso Ingress con NGINX como controlador de ingreso y especifica el uso del emisor de clúster Let's Encrypt para TLS.
- *WordPress-Deployment.yaml*: Despliega WordPress como un conjunto escalable de pods dentro del clúster, consumiendo el servicio ERDS de AWS como base de datos.
- *WordPress-PV-PVC.yaml*: Define un PersistentVolume (PV) y una PersistentVolumeClaim (PVC) para almacenamiento compartido entre los pods de WordPress usando un EFS en AWS

## Prerrequisitos

- Un clúster de Kubernetes
- kubectl configurado para comunicarse con su clúster
- Cert-manager instalado en su clúster para la gestión de certificados

## Pasos de Despliegue

1. *Gestión de Certificados*: Aplicar certificate.yaml y clusterIssuer.yaml para configurar la gestión de certificados con Let's Encrypt.
```kubectl apply -f clusterIssuer.yaml```

```kubectl apply -f certificate.yaml```
    

2. *Balanceador de Carga e Ingress*: Desplegar el load-balancing-ingress.yaml para enrutar el tráfico externo al servicio de WordPress.

```kubectl apply -f load-balancing-ingress.yaml```
    

3. *Almacenamiento Persistente*: Aplicar wordpress-pv-pvc.yaml para crear los recursos de almacenamiento persistente necesarios.

```kubectl apply -f wordpress-pv-pvc.yaml```
    

4. *Despliegue de WordPress*: Desplegar WordPress utilizando wordpress-deployment.yaml y exponerlo a través de wordpress-service.

```kubectl apply -f wordpress-deployment.yaml```
    

5. *Verificar el Despliegue*: Asegurarse de que todos los recursos estén correctamente desplegados y que el sitio de WordPress sea accesible.
```kubectl get all```
    

## Accediendo a su Sitio de WordPress

Después de que el controlador de Ingress enrute el tráfico, puede acceder a su sitio de WordPress en https://secure-page.me.

## Referencias

* https://medium.com/@seifeddinerajhi/from-http-to-https-enabling-https-only-kubernetes-ingress-with-cert-manager-ad2ff06e5b85
* https://www.youtube.com/watch?v=pumX2Ds5L0c&t=204s
* https://medium.com/@icheko/wordpress-high-availability-on-kubernetes-f6c0bcc2f28d