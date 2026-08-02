# VoiceTerm

[English](README.md) · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [Español](README.es.md)

**Colaboración de terminal guiada por voz para Codex y tmux.**

VoiceTerm permite que un asistente de programación con IA y una persona trabajen en la misma sesión local de terminal mediante `tmux`. Está pensado para el uso por voz: inicia una sesión con nombre para un proyecto y pide al asistente que inspeccione la salida o ejecute una tarea en esa sesión.

> VoiceTerm estandariza un flujo de trabajo seguro. No es una zona aislada y no puede omitir los permisos de Codex, del sistema operativo ni de la terminal.

## ¿Qué es VoiceTerm?

Puedes seguir usando iTerm2, Ghostty u otra terminal para ver y trabajar en la sesión. Tras tu aprobación, Codex puede leer la salida de la misma sesión de `tmux` y enviar comandos mediante un flujo guiado por voz.

Es útil para:

- desarrollar, ejecutar pruebas o revisar registros mientras mantienes una conversación por voz;
- trabajar con varios proyectos o varias tareas paralelas dentro de un proyecto;
- usar reglas claras de confirmación por voz para reducir acciones accidentales.

## Requisitos

- Codex con Skills disponibles.
- Un emulador de terminal capaz de ejecutar un shell.
- `tmux` instalado.

Instala tmux con un gestor de paquetes habitual de tu plataforma:

```bash
# macOS (con Homebrew)
brew install tmux

# Ubuntu / Debian / WSL Ubuntu
sudo apt install tmux

# Fedora
sudo dnf install tmux
```

En Windows, ejecuta tmux dentro de **WSL**. Windows PowerShell nativo no es por sí solo un entorno tmux; PowerShell puede usarse como shell en macOS, Linux o WSL.

## Instalar VoiceTerm

Clona este repositorio y copia la carpeta del Skill al directorio global de Skills de Codex:

```bash
git clone git@github.com:houyongsheng/voiceterm-skill.git
cd voiceterm-skill
mkdir -p ~/.codex/skills
cp -R skills/voice-term ~/.codex/skills/voice-term
```

En Windows, ejecuta los mismos comandos en WSL. Inicia una nueva tarea de Codex después de instalarlo; reinicia Codex si el Skill no se detecta.

## Inicio rápido

Abre una terminal en la raíz del proyecto objetivo y crea una sesión con el formato `proyecto-propósito`:

```bash
tmux new -s hsk3-web
```

Después di: “Usa VoiceTerm para trabajar con `hsk3-web`”.

Para conectarte a una sesión existente:

```bash
tmux attach -t hsk3-web
```

Usa sufijos de función significativos como `web`, `api`, `test`, `log` o `fix`. Si la misma función necesita otra sesión independiente, añade un número corto, por ejemplo `hsk3-test-2`.

## Varios proyectos y terminales

| Escenario | Nombre de sesión sugerido |
| --- | --- |
| Trabajo de front-end para `hsk3` | `hsk3-web` |
| Pruebas para `hsk3` | `hsk3-test` |
| Registros de servicio para `hsk3` | `hsk3-log` |
| Una segunda tarea de pruebas | `hsk3-test-2` |

VoiceTerm relaciona el proyecto y el propósito con el nombre de la sesión, y pide confirmación en lugar de adivinar cuando el destino es ambiguo.

## Modelo de seguridad

- La inspección rutinaria puede realizarse después de seleccionar la sesión objetivo.
- Los cambios en el proyecto, el control de procesos, las pruebas y la instalación de dependencias requieren una confirmación por voz clara.
- Las acciones destructivas, los cambios del historial de Git, los envíos remotos, los despliegues, los cambios de cuenta y el acceso a datos sensibles requieren una confirmación específica para cada acción.
- Las contraseñas, códigos de un solo uso, códigos de recuperación y claves de API siempre deben introducirse por la persona usuaria.
- Los avisos de permisos de Codex y del sistema operativo permanecen bajo control de la persona usuaria.
- No concedas permisos amplios por el prefijo de un comando. Autoriza los efectos, por ejemplo: solo lectura del proyecto, obtenciones públicas desde dominios indicados o el comando de pruebas de un proyecto.

## Estructura del repositorio

```text
skills/voice-term/   Skill de VoiceTerm instalable
README.md            English guide
README.zh-CN.md      简体中文指南
README.ja.md         日本語ガイド
README.es.md         Guía en español
```

## Estado

Esta es una versión inicial que se instala desde el código fuente. Se podrá añadir una distribución como plugin empaquetado después de validar el flujo de trabajo en más terminales y plataformas.
