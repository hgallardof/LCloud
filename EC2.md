# LCloud - KubeVirt: Versión Inicial (Documentación)

## Descripción General

Esta es la **versión inicial** del entorno de virtualización basado en Kubernetes usando **KubeVirt**. El objetivo es emular instancias EC2 de AWS sobre K8s, permitiendo el despliegue y gestión de máquinas virtuales (VMs) de manera local y flexible.  
En esta etapa, se trabajó principalmente con KubeVirt y almacenamiento persistente, dejando la integración avanzada de redes (Multus) para una próxima iteración.

---

## Componentes Instalados

### 1. **KubeVirt**
- **¿Qué es?**  
  KubeVirt permite ejecutar máquinas virtuales (VMs) como recursos nativos dentro de Kubernetes.
- **Instalación:**  
  Se instalaron el CRD y el operador de KubeVirt:
  ```sh
  kubectl create -f https://github.com/kubevirt/kubevirt/releases/latest/download/kubevirt-operator.yaml
  kubectl create -f https://github.com/kubevirt/kubevirt/releases/latest/download/kubevirt-cr.yaml
  ```

### 2. **Longhorn (Almacenamiento)**
- **¿Qué es?**  
  Longhorn es un sistema de almacenamiento distribuido para Kubernetes, utilizado aquí para los discos persistentes de las VMs.
- **Uso:**  
  Se crearon PersistentVolumeClaims (PVC) y PersistentVolumes (PV) usando la storageClass `longhorn`.

### 3. **virtctl**
- **¿Qué es?**  
  Herramienta de línea de comandos para interactuar con VMs de KubeVirt (conexión VNC, carga de imágenes, etc.).
- **Ejemplo de uso:**  
  - Subir una imagen ISO a un PVC:
    ```sh
    virtctl image-upload \
      --image-path=/Users/hugo/Downloads/Win10_22H2_Spanish_x64v1.iso \
      --pvc-name=iso-win2k12 \
      --access-mode=ReadOnlyMany \
      --pvc-size=5G \
      --uploadproxy-url=https://10.103.229.179:443 \
      --insecure \
      --wait-secs=240
    ```
  - Conexión VNC a una VM:
    ```sh
    virtctl vnc vmi-koandina
    ```

---

## Recursos y Objetos de Kubernetes

### **PersistentVolume y PersistentVolumeClaim**
- Se crearon recursos para almacenar imágenes y discos de las VMs.
- Ejemplo:
  ```yaml
  apiVersion: v1
  kind: PersistentVolume
  metadata:
    name: pv-iso-win10
    labels:
      type: local
  spec:
    storageClassName: longhorn
    capacity:
      storage: 15Gi
    accessModes:
      - ReadWriteMany
    hostPath:
      path: "/tmp/hostImages/win10"
  ```

### **VirtualMachineInstance (VMI)**
- Se desplegó una VM de ejemplo (`vmi-koandina`) con:
  - 4 vCPUs, 6Gi de RAM
  - Discos: ISO de Windows, disco de datos, disco VirtIO
  - Red en modo `masquerade` (NAT, acceso a Internet)
  - Configuración de DNS personalizada (primario y secundario)
- Ejemplo:
  ```yaml
  apiVersion: kubevirt.io/v1
  kind: VirtualMachineInstance
  metadata:
    name: vmi-koandina
    labels:
      special: koandina
  spec:
    domain:
      cpu:
        cores: 4
      devices:
        disks:
        - name: virtio2
          cdrom: { bus: sata }
        - name: virtio
          cdrom: { bus: sata }
        - name: pvcdisk
          disk: { bus: sata }
        interfaces:
        - name: default
          masquerade: {}
          model: virtio
        tpm: {}
      features:
        acpi: {}
        apic: {}
        hyperv:
          relaxed: {}
          spinlocks: { spinlocks: 8191 }
          vapic: {}
        smm: {}
      firmware:
        bootloader:
          efi:
            secureBoot: true
        uuid: 5d307ca9-b3ef-428c-8861-06e72d69f223
      resources:
        requests:
          memory: 6Gi
    networks:
    - name: default
      pod: {}
    dnsPolicy: None
    dnsConfig:
      nameservers:
        - 1.1.1.1
        - 8.8.8.8
    terminationGracePeriodSeconds: 0
    volumes:
    - name: pvcdisk
      persistentVolumeClaim:
        claimName: disk-windows-shared
    - name: virtio2
      persistentVolumeClaim:
        claimName: virtio.iso
    - name: virtio
      containerDisk:
        image: kubevirt/virtio-container-disk
  ```

---

## Notas y Próximos Pasos

- **Redes avanzadas (Multus):**  
  Aunque Multus fue instalado, en esta versión inicial no se configuraron redes adicionales.  
  En la próxima iteración se probarán redes secundarias y bridges personalizados para mayor flexibilidad de red en las VMs.
- **Compatibilidad:**  
  Se recomienda verificar que las imágenes de disco sean compatibles con KVM/QEMU y que los drivers VirtIO estén presentes para un mejor rendimiento.
- **Automatización:**  
  Se sugiere crear scripts o manifiestos Helm/Kustomize para facilitar la reproducción del entorno.

---

## Resumen

- Se instaló y configuró KubeVirt sobre Kubernetes.
- Se preparó almacenamiento persistente con Longhorn.
- Se desplegó una VM de ejemplo con acceso a Internet y DNS personalizado.
- **Próximamente:** integración avanzada de redes con Multus.

---

**Versión inicial - LCloud KubeVirt**  
*Para dudas o mejoras, contribuye en el repositorio o abre un issue.*
