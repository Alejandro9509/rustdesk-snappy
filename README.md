# RustDesk para Snap

Este proyecto empaqueta el cliente Linux de [RustDesk](https://github.com/rustdesk/rustdesk)
como un snap. Usa `core26` tanto para compilar como para ejecutar, e incluye la lista de
dependencias nativas indicada por RustDesk más las herramientas adicionales de su versión
Flutter.

Aunque la guía de RustDesk incluye dependencias para Ubuntu 18, Snapcraft 9.2.1 ya no
admite `core18` como entorno de compilación. Por eso esta receta adapta las dependencias
al entorno `core26`; no compila sobre Ubuntu 18.

La receta sigue la rama `master` de RustDesk. Fija Rust 1.75.0, Flutter 3.24.5 y lee
la versión de RustDesk desde su `Cargo.toml`. Por eso el paquete puede cambiar al
reconstruirse, aunque no cambie este proyecto.

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

## Instalar para pruebas

La receta está marcada `devel` y `devmode`, por lo que está pensada para pruebas locales:

```bash
sudo snap install --dangerous --devmode ./rustdesk-snappy_<version>_amd64.snap
```

El confinamiento `devmode` evita prometer aislamiento y compatibilidad con Snap Store.
Las interfaces declaradas documentan los permisos que necesitaría una futura variante
confinada; el acceso real a captura de pantalla, entrada y audio depende también del
servidor gráfico y de las conexiones de interfaces disponibles en el sistema.
