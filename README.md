# Instalar tortoise-tts

Tortoise TTS es una herramienta de síntesis de voz de código abierto desarrollada por [neonbjb](https://github.com/neonbjb). Utiliza modelos de lenguaje de última generación para convertir texto en voz de forma rápida y sencilla. Con Tortoise TTS, puedes generar voces humanas realistas en varios idiomas y estilos.

<br>

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

<br>

## Paso 2: Conectarse a la VM

1. Conéctate mediante SSH:
   - Una vez creada la instancia, haz clic en SSH para conectarte a la VM desde el navegador.

<br>

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

<br>

## Paso 4: Configurar acceso remoto desde tu ordenador

1. **Instalar `flask` para crear una API simple:**

   ```sh
   pip install flask
   ```

2. **Ir a la carpeta**

   ```sh
   cd tortoise-tts
   ```

3. **Crear Carpeta para la API**

   ```sh
   mkdir api_tortoise
   cd api_tortoise
   ```

4. **Crear Carpeta para la API**

   ```sh
   nano app.py
   ```

5. **Añade el siguiente código:**

   ```python
   from flask import Flask, request, send_file
   import subprocess

   app = Flask(__name__)

   @app.route('/synthesize', methods=['POST'])
   def synthesize():
      text = request.form['text']
      output_file = 'output.wav'
      subprocess.run(['python3', 'tortoise/do_tts.py', '--text', text, '--voice', 'random', '--preset', 'fast', '--output_path', output_file])
      return send_file(output_file, as_attachment=True)

   if __name__ == '__main__':
      app.run(host='0.0.0.0', port=5000)
   ```

6. **Guardar y salir del archivo**

   Para guardar y salir del editor de texto Nano, puedes seguir estos pasos:

   - Presiona `Ctrl + O` para guardar el archivo.
   - Aparecerá una línea en la parte inferior de la pantalla donde puedes confirmar el nombre del archivo. Simplemente presiona `Enter` para confirmar el nombre actual del archivo (`app.py`).
   - Luego, presiona `Ctrl + X` para salir de Nano.

7. **Ejecutar el servidor Flask:**
   ```sh
   cd ~/tortoise-tts/api_tortoise
   python3 app.py
   ```

<br>

## Paso 5: Activar todo cada vez que se entra

1. **Activar entorno virtual:**

   ```sh
   source ~/tortoise-tts/venv/bin/activate
   ```

2. **Activar API flask**

   ```sh
   python3 ~/tortoise-tts/api_tortoise/app.py
   ```

<br>

### Paso 6: Enviar texto desde tu ordenador y obtener el audio

1. **Enviar texto mediante `curl` desde tu ordenador:**
   ```sh
   curl -X POST -F "text=Hola, esto es una prueba de Tortoise TTS." http://34.125.77.15:5000/synthesize --output output.wav
   ```
