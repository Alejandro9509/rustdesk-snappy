# RustDesk para Snap

Este proyecto empaqueta el cliente Linux de [RustDesk](https://github.com/rustdesk/rustdesk)
como un snap confinado en modo `strict`, preparado para publicarse en Snap Store. Usa
`core26` tanto para compilar como para ejecutar, e incluye la lista de dependencias nativas
indicada por RustDesk más las herramientas adicionales de su versión Flutter.

Aunque la guía de RustDesk incluye dependencias para Ubuntu 18, Snapcraft 9.2.1 ya no
admite `core18` como entorno de compilación. Por eso esta receta adapta las dependencias
al entorno `core26`; no compila sobre Ubuntu 18.

La receta sigue la rama `master` de RustDesk. Fija Rust 1.75.0, Flutter 3.24.5,
Flutter Rust Bridge 1.80.1 y lee la versión de RustDesk desde su `Cargo.toml`. Genera
los archivos bridge antes de compilar; por eso el paquete puede cambiar al reconstruirse,
aunque no cambie este proyecto.

## Compilar

En Linux, instala Snapcraft y configura LXD para que Snapcraft compile usando el entorno
aislado declarado en `snap/snapcraft.yaml`:

```bash
sudo snap install snapcraft --classic
sudo snap install lxd
sudo lxd init --auto
snapcraft --use-lxd
```

La compilación descarga RustDesk y sus submódulos, Flutter, Rust, CMake 4.3.0 y vcpkg;
además compila las dependencias multimedia. Necesita conexión a Internet, espacio y
memoria suficientes.
La receta actual produce paquetes `amd64`.

## Publicar

Después de compilar, sube el paquete a Snap Store y solicita su publicación en `stable`:

```bash
snapcraft upload --release=stable rustdesk-snappy_<version>_amd64.snap
```

La publicación queda sujeta a la revisión y aprobación de Snap Store. En particular,
`raw-input` y `uinput` son permisos sensibles para el control remoto y pueden requerir
revisión o conexión explícita en los sistemas donde se instale el snap.

## Probar el paquete confinado localmente

Para instalar el paquete recién compilado antes de que esté disponible en la tienda, se
puede cargar como sideload. `--dangerous` omite la verificación de procedencia de la tienda,
pero el snap conserva el confinamiento `strict`:

```bash
sudo snap install --dangerous ./rustdesk-snappy_<version>_amd64.snap
```

En Ubuntu 26, instala y conecta el proveedor de gráficos para habilitar OpenGL:

```bash
sudo snap install mesa-2604 --channel=26/stable
sudo snap connect rustdesk-snappy:gpu-2604 mesa-2604:gpu-2604
```

El funcionamiento de captura de pantalla, entrada y audio también depende del servidor
gráfico y de las conexiones de interfaces disponibles en el sistema.
