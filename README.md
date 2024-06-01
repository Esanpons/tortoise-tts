# Instalar tortoise-tts

## Paso 1: Crear una instancia de VM en Google Cloud

1. Accede a Google Cloud Console: Ve a [Google Cloud Console](https://console.cloud.google.com/).

2. Crea una nueva instancia de VM:

   - En el menú de la izquierda, selecciona Compute Engine y luego VM instances.
   - Haz clic en Create instance.

3. Configura los detalles de tu instancia:

   - Nombre: `tortoise-tts-vm`
   - Región y zona: Selecciona la que prefieras.
   - Tipo de máquina: Escoge una con suficiente CPU y memoria. Una `n1-standard-4` debería ser suficiente.
   - Sistema operativo: Elige una imagen basada en Ubuntu (por ejemplo, Ubuntu 20.04 LTS).

4. Permitir tráfico HTTP/HTTPS:

   - En la sección de Firewall, marca las casillas para permitir tráfico HTTP y HTTPS.

5. Haz clic en Create.

## Paso 2: Conectarse a la VM

1. Conéctate mediante SSH:
   - Una vez creada la instancia, haz clic en SSH para conectarte a la VM desde el navegador.

## Paso 3: Configurar el entorno en la VM

1. Actualizar el sistema

```bash
sudo apt update
sudo apt upgrade -y
```

2. Instalar dependencias esenciales

```bash
sudo apt install -y git python3 python3-venv python3-pip ffmpeg
```

3. Clonar el repositorio y acceder a él

```bash
git clone https://github.com/neonbjb/tortoise-tts.git
cd tortoise-tts
```

4. Crear y activar un entorno virtual

```bash
python3 -m venv venv
source venv/bin/activate
```

5. Instalar las dependencias requeridas

```bash
pip install -r requirements.txt
```

6. Desinstalar tokenizers si está instalado

```bash
pip uninstall tokenizers
```

7. Instalar la versión específica de transformers

```bash
pip install transformers==4.31.0
```

8. Instalar la versión requerida de tokenizers

```bash
pip install tokenizers==0.13.3
```

9. Establecer la variable PYTHONPATH

```bash
export PYTHONPATH=$(pwd)
```

10. Ejecutar el script de Tortoise TTS

```bash
python tortoise/do_tts.py --text "Hola, esto es una prueba de Tortoise TTS." --voice "random" --preset "fast"
```
