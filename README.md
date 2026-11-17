# Hyperion

Chat con IA que corre entero en tu ordenador: sin nube, sin cuenta y sin red salvo que tú la actives.
Piensa solo cuando hace falta (lo cotidiano lo responde al momento), consulta una enciclopedia local, ejecuta programas en una microVM aislada y
lleva tareas largas en una carpeta de trabajo. Cerebro: Qwen3.5-9B (IQ4_XS, con su cabeza MTP).

Las descargas están en [Releases](../../releases). Este repositorio solo publica el paquete.

## Requisitos

- **Windows 10/11 con WSL2** (`wsl --install` en PowerShell como administrador) o **Linux x86-64 con KVM** y glibc ≥ 2.39 (Ubuntu 24.04 o posterior).
- Virtualización activada en la BIOS/UEFI. En WSL2, la virtualización anidada (la trae activada Windows 11 en los equipos que la admiten).
- 16 GB de RAM y una GPU NVIDIA con 8 GB de VRAM (sin GPU funciona en CPU, mucho más lento).
- ≈ 16 GB libres durante la instalación; instalado ocupa 6,2 GiB. Cada paquete de biblioteca suma lo que ocupa (tabla de abajo) y pide el doble libre al instalarlo.

El instalador lo comprueba todo antes de tocar nada; si falta algo, dice cómo arreglarlo y no deja nada instalado.

## Instalar

**Windows** (PowerShell, sin administrador): crea una distro de WSL propia, «Hyperion», sin tocar las tuyas, y pone el icono «Hyperion» en el escritorio y en el menú Inicio.

```powershell
irm https://github.com/samuelpichelgordillo-dot/hyperion/releases/latest/download/install.ps1 -OutFile $env:TEMP\hyperion-install.ps1
powershell -ExecutionPolicy Bypass -File $env:TEMP\hyperion-install.ps1
```

Opciones: `-Biblioteca` (añade los hechos de Wikidata en español, `wd-es`), `-Paquetes wd-es,wd-en,eswiki,enwiki` (los paquetes de biblioteca que quieras, ver abajo), `-Ubicacion D:\Hyperion` (otro disco), `-Cpu`, `-Desinstalar`.

**Linux** (como root o con sudo, por las microVMs):

```bash
curl -fLO https://github.com/samuelpichelgordillo-dot/hyperion/releases/latest/download/install.sh
bash install.sh
```

Opciones: `--with <paquete>` (una vez por paquete de biblioteca, ver abajo), `--cpu`, `--help`. Después: `hyperion chat` (terminal), `hyperion ui` (navegador), `hyperion local` (comprobación), `hyperion uninstall`.

## Biblioteca (opcional)

Hyperion consulta la biblioteca él solo cuando le preguntas un dato y duda, o cuando tiene algo relevante; no sale de tu equipo. Cuanta más biblioteca, menos inventa en los datos raros.

| paquete | qué es | instalado | licencia |
|---|---|---:|---|
| `wd-es` | fichas de Wikidata de las entidades con artículo en la Wikipedia en español | 1,08 GiB | CC0 |
| `wd-en` | fichas de Wikidata de las que solo tienen artículo en inglés | 2,89 GiB | CC0 |
| `eswiki` | la Wikipedia en español entera (1,7 M artículos) | 1,67 GiB | CC BY-SA 4.0 |
| `enwiki` | las entradillas de la Wikipedia en inglés (6,4 M artículos) | 3,84 GiB | CC BY-SA 4.0 |

Los cuatro juntos: 9,5 GiB. Se pueden añadir después volviendo a lanzar el instalador con ellos.

Los modelos (Qwen3.5-9B, nomic-embed-text-v2-moe), Firecracker, el kernel de las microVMs y Alpine se bajan de su origen con la versión fijada y el hash comprobado; el instalador lleva dentro los hashes de todo. Volver a lanzarlo actualiza sin bajar otra vez lo que no ha cambiado.

## Qué se descarga y licencias

Cada versión publica `HUELLA.md` (tamaño y licencia de cada componente) y `SHA256SUMS`. Los programas son Apache-2.0 (avisos de las dependencias en `share/THIRD_PARTY.md` dentro del paquete); el cerebro y los embeddings, Apache-2.0; la enciclopedia local y los paquetes `eswiki` y `enwiki` derivan de la Wikipedia (CC BY-SA 4.0, atribución en `share/ATRIBUCION.md`); los de Wikidata son CC0. El kernel (GPL-2.0) y la imagen de las microVMs no se redistribuyen: se bajan de su origen o se construyen al instalar.
