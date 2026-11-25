# RustDesk para Snap

[![Licencia de RustDesk: AGPL-3.0-only](https://img.shields.io/badge/RustDesk-AGPL--3.0--only-blue.svg)](https://github.com/rustdesk/rustdesk/blob/master/LICENCE)

**RustDesk** es una aplicación de escritorio remoto que permite conectarse a otros
equipos y controlarlos con autorización. Este paquete ofrece el cliente Linux de RustDesk
como un Snap confinado en modo `strict`, basado en el
[proyecto original](https://github.com/rustdesk/rustdesk).

## Características

- Conexión remota entre equipos.
- Transferencia de archivos y uso compartido del portapapeles durante una sesión.
- Posibilidad de usar la infraestructura de RustDesk o configurar servidores propios.
- Interfaz Linux basada en Flutter.

La disponibilidad de captura de pantalla, entrada, audio y otras funciones depende del
entorno gráfico y de los permisos de Snap conectados en el sistema.

## Instalación local

Si tienes el archivo Snap en tu equipo, instálalo desde el directorio donde se encuentra:

```bash
sudo snap install --dangerous ./rustdesk-snappy_<version>_amd64.snap
```

`--dangerous` permite instalar un archivo local que no está firmado por Snap Store. El
paquete conserva el confinamiento `strict`; no añadas `--devmode`.

En Ubuntu 26, el proveedor gráfico de Mesa puede habilitar la integración con GPU:

```bash
sudo snap install mesa-2604 --channel=26/stable
sudo snap connect rustdesk-snappy:gpu-2604 mesa-2604:gpu-2604
```

RustDesk declara permisos de red, audio, escritorio y entrada. En particular, `raw-input`
y `uinput` se usan para funciones de control remoto y pueden requerir autorización de
Snap Store o conexión explícita. Para consultar las conexiones disponibles:

```bash
snap connections rustdesk-snappy
```

## Seguridad y uso responsable

Usa RustDesk únicamente para acceder a equipos y sesiones para los que tengas autorización.
El confinamiento `strict` limita el acceso del Snap al sistema, pero las funciones
disponibles también dependen de las interfaces conectadas y de la configuración del
escritorio.

## Proyecto y licencia

- [Sitio oficial de RustDesk](https://rustdesk.com)
- [Código fuente de RustDesk](https://github.com/rustdesk/rustdesk)
- [Licencia AGPL-3.0-only de RustDesk](https://github.com/rustdesk/rustdesk/blob/master/LICENCE)

La aplicación RustDesk incluida en este Snap se distribuye bajo la licencia
[GNU Affero General Public License, versión 3](https://www.gnu.org/licenses/agpl-3.0.html).
Consulta el archivo `LICENCE` del proyecto original para ver el texto íntegro, sus
condiciones y los avisos correspondientes.
