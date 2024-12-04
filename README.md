# Web Scraping con Selenium

Bienvenidos al proyecto sobre web scraping. Este repositorio contiene el material necesario para entender, instalar y trabajar con selenium, una potente herramienta de automatización diseñada para pruebas funcionales de aplicaciones web. Contiene una solución para web scraping y automatización utilizando contenedores Docker y Selenium Grid.

## Estructura del Repositorio

```
.
├── notebooks/         # Jupyter notebooks para la clase
├── tareas/            # Directorio de tareas
├── Dockerfile         # Configuración del contenedor
├── README.md          # Este archivo
└── requirements.txt   # Dependencias del proyecto
```

## Instalación

Crear un fork y clonar tu propio repositorio
```bash
cd path/donde/quieres/tu/repositorio
git clone url/git@
cd web-scraping
```

## Red de Contenedores

Una red de contenedores es un entorno virtualizado que conecta contenedores entre sí y con otros recursos, como bases de datos, servicios externos o redes físicas.

### Tipos de Red

- **Bridge (puente)**:
  - La más común para contenedores en una misma máquina
  - Se utiliza por defecto si no especificas otra red
  - Los contenedores conectados a esta red pueden comunicarse entre sí usando nombres de host

## Configuración del Ambiente

### 1. Crear la Red

Crea la red de contenedores que conectará el servidor Selenium con el cliente:

```bash
docker network create selenium-network
```

### 2. Configurar Selenium Server

Descarga y ejecuta el servidor Selenium con Chrome:

```bash
# Descargar la imagen
docker pull selenium/standalone-chrome
o
docker pull seleniarm/standalone-chromium # Para Mac M1, M2, M3                                               

# Ejecutar el servidor
docker run -d --name selenium-server --network selenium-network -p 4444:4444 selenium/standalone-chrome
o
docker run -d --name selenium-server --network selenium-network -p 4444:4444 seleniarm/standalone-chromium # Para Mac M1, M2, M3
```

Para abrir el contenedor primero descarga en VS Code la extensión de Docker e identifica el siguiente apartado:  
<img width="527" alt="Screenshot 2024-12-04 at 12 39 09" src="https://github.com/user-attachments/assets/d511494e-905f-4482-8f9d-24f8eb952767">

Considera el container con nombre "selenium/standalone-chrome" o "seleniarm/standalone-chromium" y luego deberas hacer doble click. Si no esta iniciado, deberas hacer click en "start". Después harás nuevamente doble click y harás click en "Open in Browser". Esto abrirá la ventana en chrome que funcionara como pantalla. 
<img width="230" alt="Screenshot 2024-12-04 at 12 41 25" src="https://github.com/user-attachments/assets/43354c06-9310-43ff-975f-3a028f23a089">

### 3. Configurar Visualización (X11)

Para visualizar las ventanas del navegador, configura X11:

```bash
# Verificar que X11 está activo. 
echo $DISPLAY
```

Si devuelve algo como :0 o :1, significa que X11 está configurado correctamente. Si no devuelve nada, necesitas iniciar X11. En Ubuntu, X11 normalmente está preinstalado, pero si no lo está, instálalo con:
```bash
# Si X11 no está instalado, instálalo
sudo apt update && sudo apt install x11-apps -y
xhost +local: # # Permitir conexiones locales a X11
```



### 4. Configurar Cliente Selenium con Jupyter

Construir la imagen del cliente
```bash
# Construir la imagen del cliente
docker build -t selenium-client .
```

Ejecutar el cliente
```bash
docker run --rm -it \
    --name selenium-client \
    --network selenium-network \
    -p 8888:8888 \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    selenium-client
```

## Uso

1. Asegúrate de que todos los contenedores estén ejecutándose correctamente
2. Accede a Jupyter Notebook a través de http://localhost:8888
3. Utiliza los notebooks en el directorio `notebooks/` para ejecutar tus tareas de scraping
