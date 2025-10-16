Este es un modelo de una red neuronal que traduce Lengua de Señas Peruana (LSP) a texto (y voz). Utilicé MediaPipe para obtener los puntos de la seña y para el entrenamiento usé TensorFlow y Keras.

## SCRIPTS PRINCIPALES
- capture_samples.py → captura las muestras y las ubica en la carpeta frame_actions.
- normalize_samples.py → normaliza las muestras para que todas tengan la misma cantidad de frames (importante).
- create_keypoints.py → crea los keypoints que se usarán en el entrenamiento.
- training_model.py → entrena la red neuronal.
- evaluate_model.py → donde se realiza la prueba de la red neuronal.
- main.py → donde se utiliza una GUI para usar el traductor.

## SCRIPTS SECUNDARIOS
- model.py → aquí se ajusta el modelo de la red neuronal.
- constants.py → ajustes de la red neuronal.
- helpers.py → funciones que se utilizan en los scripts principales.

## Pasos para probar la red neuronal
1. Capturar las muestras con capture_samples.py
2. Normalizar las muestras con normalize_samples.py
2. Generar los .h5 (keypoints) de cada palabra con create_keypoints.py
3. Entrenar el modelo con training_model.py
4. Realizar pruebas con evaluate_model.py

## Video de la explicación del código:
https://youtu.be/3EK0TxfoAMk
Nota: Pronto subiré otro video explicando las mejoras.

## Requisitos e instalación (Windows)

- **Python**: 3.10 (recomendado, 64-bit)
- **SO**: Windows 10/11 (64-bit)
- **Dependencias del sistema**: Microsoft Visual C++ Redistributable 2015-2022
- **Hardware**: Cámara web, micrófono y parlantes
- **Internet**: Requerido para `gTTS` (texto a voz)

### Crear entorno virtual e instalar dependencias

```powershell
# 1) Crear y activar entorno (ajusta a tu versión si no tienes py -3.10)
py -3.10 -m venv .venv
.\.venv\Scripts\activate

# 2) Actualizar pip
python -m pip install --upgrade pip

# 3) Instalar requisitos del proyecto
pip install -r requirements.txt
```

Si tienes problemas con TensorFlow, asegúrate de usar Python 3.10 x64.

## Estructura de carpetas y archivos

- **`frame_actions/`**: aquí se guardan las capturas de frames por palabra.
- **`data/keypoints/`**: aquí se generan los `.h5` (keypoints) por palabra.
- **`models/`**: contiene el modelo entrenado (`actions_15.keras`) y `words.json`.

Estas carpetas se crean automáticamente según se ejecutan los scripts. Si no existen, créalas manualmente.

## Comandos para ejecutar el proyecto

Ejecuta todos los comandos desde la carpeta del proyecto `modelo_lstm_lsp-main/` con el entorno virtual activado.

- **1) Capturar muestras** (`capture_samples.py`)
  - Edita `word_name` en `capture_samples.py` para la palabra a capturar.
  - Ejecuta:
    ```powershell
    python capture_samples.py
    ```
  - Sal con la tecla `q`.

- **2) Normalizar frames a longitud fija** (`normalize_samples.py`)
  ```powershell
  python normalize_samples.py
  ```

- **3) Generar keypoints (.h5) por palabra** (`create_keypoints.py`)
  - Genera para todas las palabras presentes en `frame_actions/`:
    ```powershell
    python create_keypoints.py
    ```

- **4) Entrenar el modelo** (`training_model.py`)
  - Entrena y guarda en `models/actions_15.keras` (por defecto):
    ```powershell
    python training_model.py
    ```

- **5) Evaluar en tiempo real (consola/ventana OpenCV)** (`evaluate_model.py`)
  ```powershell
  python evaluate_model.py
  ```

- **6) GUI del traductor (PyQt5)** (`main.py`)
  ```powershell
  python main.py
  ```

## Notas y tips

- **Audio (gTTS/pygame)**: requiere conexión a internet y salida de audio disponible.
- **Cámara ocupada**: cierra apps que usen la cámara si no abre.
- **Rendimiento**: baja la resolución/FPS en `main.py` o `evaluate_model.py` si tu PC es lenta.
- **Modelo/Palabras**: `models/words.json` define el orden de clases. Asegúrate de que las palabras capturadas coincidan con este archivo o actualízalo acorde a tus datos.
