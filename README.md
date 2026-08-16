# 👁️ ARGOS NETWORKS
### Infraestructura de Red Empresarial · Proyecto Final de Redes

> *"Argos Panoptes, el guardián de los cien ojos, nunca dormía por completo: mientras unos ojos descansaban, otros permanecían siempre despiertos, vigilando."*
> — Mitología griega

---

## 👁️‍🗨️ Filosofía: el Panóptico de Argos

En la mitología griega, **Argos Panoptes** era un gigante cubierto de cien ojos, capaz de observar todo lo que ocurría a su alrededor sin nunca perder la vigilancia por completo. Hera lo puso a custodiar lo que más valor tenía para ella, precisamente porque nada escapaba a su mirada.

**Argos Networks** toma ese mismo principio como razón de ser: una infraestructura no es segura ni confiable si no puede ser *vista*. Cada VLAN, cada túnel VPN, cada sede remota, cada usuario conectado, es un "ojo" dentro de una red que debe permanecer despierta, trazable y coherente incluso cuando opera de forma distribuida entre cinco ciudades del país.

De esa filosofía nace **ARGOS EYE**, nuestra plataforma de visibilidad: no basta con que la red funcione, tiene que poder *contar su propia historia* — qué pasó, dónde, cuándo y por qué — convirtiendo datos dispersos de la infraestructura en información trazable para la toma de decisiones.

Este repositorio documenta la infraestructura que sostiene esa visión: la columna vertebral técnica detrás del ojo que todo lo ve.

---

## 🏢 Sobre la empresa

**Argos Networks** es una empresa dominicana especializada en atención al cliente, comercio electrónico, soporte tecnológico y comunicaciones empresariales, con presencia física en **Santo Domingo (sede central), Santiago, Puerto Plata, La Romana y Barahona**.

### Misión
Impulsar la transformación digital de las empresas dominicanas ofreciendo comercio electrónico, soporte tecnológico y comunicaciones seguras. Garantizamos operatividad continua y trazabilidad mediante nuestra red nacional y la plataforma ARGOS EYE.

### Valores

| Valor | Descripción |
|---|---|
| 🔒 **Fiabilidad** | Conectividad continua, estable y segura en todas nuestras sedes. |
| 💡 **Innovación** | Convertimos datos dispersos en información clara y accionable con ARGOS EYE. |
| 🤝 **Excelencia en Servicio** | Soporte oportuno, enfocado en la satisfacción del cliente. |
| 🇩🇴 **Presencia Nacional** | Conectamos estratégicamente las principales regiones del país. |
| 🔍 **Transparencia** | Trazabilidad y documentación clara en cada evento e incidente. |

---

## 🗺️ Arquitectura de la red

La red está diseñada bajo **OSPF multi-área**, con Santo Domingo como **HUB** (Área 0 / Área 1) y las demás sedes como **spokes** conectados vía **VPN IPSec site-to-site**.

| Sede | Área OSPF | Función principal | VLANs |
|---|---|---|---|
| ISP | Backbone (Área 0) | Tránsito entre sedes | — |
| Santo Domingo | Área 0 / Área 1 | HUB de VPN, doble multicapa con HSRP + VTP Server | Administración, RRHH, Marketing, Ventas |
| Santiago | Área 5 | Servidor multiservicio Ubuntu (DNS/DHCP/WEB/NFS/RADIUS/Correo) | DataCenter, Ventas, Administración |
| Puerto Plata | Área 2 | Sucursal (spoke) | Tecnología, Contabilidad |
| La Romana | Área 4 | Sucursal (spoke), ACL de segmentación | Tecnología, Contabilidad, Finanzas |
| Barahona | Área 3 | Sucursal (spoke), ACL de segmentación | Tecnología, Contabilidad |

**Tecnologías implementadas:**
- Enrutamiento: OSPF multiárea (ABR en Santo Domingo y Santiago)
- Alta disponibilidad: HSRP en la capa de distribución de Santo Domingo
- Distribución de VLAN: VTP v2 (servidor/cliente)
- Segmentación L2: EtherChannel (LACP), 802.1Q trunking, VLAN nativa dedicada (100)
- Seguridad de capa 2: DHCP Snooping, Dynamic ARP Inspection, Port-Security
- Gestión remota: SSH v2, ACLs de administración, usuarios con privilegios diferenciados
- Interconexión de sedes: VPN IPSec (IKEv1, AES-256, SHA-256, PFS grupo 14)
- Servicios: DNS, DHCP (con *ip helper-address* / relay), Web, NFS, RADIUS y Correo sobre Ubuntu Server

---

## 📁 Estructura del repositorio

```
argos-networks-infraestructura/
│
├── README.md                          ← este archivo
├── LICENSE
├── .gitignore
│
├── docs/
│   ├── ARQUITECTURA.md                ← diagrama y explicación de direccionamiento IP
│   ├── ARGOS-EYE.md                   ← filosofía y visión de la plataforma de visibilidad
│   └── VERIFICACION.md                ← comandos de verificación final (show ip ospf, etc.)
│
├── configs/
│   ├── 00-isp/
│   │   └── isp.txt
│   │
│   ├── 01-santo-domingo/
│   │   ├── router-santo-domingo.txt
│   │   ├── r-sw1-vtp-server.txt
│   │   ├── r-sw2-vtp-cliente.txt
│   │   ├── sw-administracion.txt
│   │   ├── sw-rrhh.txt
│   │   └── sw-ventas-marketing.txt
│   │
│   ├── 02-puerto-plata/
│   │   ├── router-puerto-plata.txt
│   │   ├── sw-tecnologia.txt
│   │   └── sw-contabilidad.txt
│   │
│   ├── 03-barahona/
│   │   ├── router-barahona.txt
│   │   ├── sw1-barahona.txt
│   │   └── sw2-barahona.txt
│   │
│   ├── 04-la-romana/
│   │   ├── router-la-romana.txt
│   │   ├── sw1-romana.txt
│   │   └── sw2-romana.txt
│   │
│   └── 05-santiago/
│       ├── multicapa-santiago.txt
│       └── sw1-santiago.txt
│
└── server/
    └── setup_servidor_santiago.sh     ← script de aprovisionamiento del servidor Ubuntu
```

---


<p align="center"><i>"Cien ojos, una sola red."</i> 👁️</p>
