---

copyright:

  years:  2016, 2019

lastupdated: "2019-01-04"

---

{:tip: .tip}
{:note: .note}
{:important: .important}

# Visión general de KMIP for VMware

Esta arquitectura de la solución describe la arquitectura de KMIP on VMware para proteger las instancias de VMware on {{site.data.keyword.cloud_notm}}. Existen muchas opciones de cifrado de almacenamiento disponibles para proteger la carga de trabajo de VMware. KMIP for VMware trabaja junto con el cifrado de vSphere nativo de VMware y el cifrado de vSAN para proporcionar una gestión de cifrado de almacenamiento simplificada junto con la seguridad y la flexibilidad de las claves gestionadas por el cliente de {{site.data.keyword.cloud_notm}} Key Protect.

Se considera que esta solución es un componente adicional y una extensión de las ofertas de soluciones de vCenter Server y VMware Cloud Foundation en {{site.data.keyword.cloud_notm}}. Como resultado, este documento no cubre la configuración existente de estas soluciones de base en {{site.data.keyword.cloud_notm}}. Para comprender mejor la arquitectura de la solución de base, consulte nuestra [arquitectura de la solución VMware on {{site.data.keyword.cloud_notm}}](../solution/solution_overview.html).

## Beneficios clave

Aunque hay muchas soluciones de cifrado de almacenamiento disponibles para la carga de trabajo de VMware, KMIP for VMware ofrece las ventajas siguientes:

* Integración con el cifrado de VMware VSAN y con el cifrado de vSphere, que se implementan en la capa del hipervisor en lugar de implementarse en la capa de almacenamiento o de máquina virtual. Este enfoque facilita la gestión y ofrece transparencia en la aplicación y en la solución de almacenamiento.
* Servidor de gestión de claves totalmente gestionado disponible en muchas regiones multizona (MZR) de IBM Cloud.
* La integración del clúster de VMware con IBM Cloud Key Protect le proporciona claves totalmente gestionadas por el cliente que puede revocar en cualquier momento.

### Enlaces relacionados

* [Diseño de la solución](design.html)
* [Implementación y gestión](implementation.html)