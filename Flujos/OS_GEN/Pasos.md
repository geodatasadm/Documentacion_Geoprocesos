# Obtencion de Datos con SQL SERVER
1. Abrir SQL server
2. Continuara...

# Conexion a la tabla de SQL SERVER CON ARCMAP
1. En Arcmap, ir a database conections

2. Elegir la siguiente base de datos y luego la respectiva tabla señalada<br><img width="319" height="173" alt="image" src="https://github.com/user-attachments/assets/455414b4-8af7-4123-a2d4-b958b3b730e7" />
<img width="407" height="159" alt="image" src="https://github.com/user-attachments/assets/70fff15c-341a-48b9-beda-cf5c5a8f21ee" />

3. Exportar tabla a gdb dejando las rutas que se coemtaron en la instruccion y dejando todos los campos por default
   
   <img width="994" height="680" alt="image" src="https://github.com/user-attachments/assets/2340143b-79f2-4313-b5fc-92d343084cf0" />

# Depuracion de tabla recien exportada
1. Se consideraran los siguientes factores para depurar la tabla con la que posteriormente se creara el feature class de puntos, basicamente se tomara en cuenta solo 1 año atras de dia que se este realizando la actualizacion y solo se dejaran los registros que cuenten con latitud y longitud completos.
A continuacion se obervara el query de ejemplo:
```sql
fecha_resolucion >= date '2024-11-25 12:00:00' AND latitud >1 AND longitud <-1
```

2. Posteriormente se volvera a exportar dicha tabla con el query en la misma gdb en la cual se esta trabajando, se le puede nombrar tabla_dep

# Crecion de capa OS_GEOG y OS_GEN
1. A patir de la tabla depurada crear una capa de puntos a paritr de sus atributos de latitud y longitd, de la capa temporal resultante, exportarla y nombrarla OS_GEOG porque dicha capa esta hecha a partir de coordenadas geograficas.
2. Despues mediante la herramineta Project, hacer la proyeccion de la capa OS_GEOG a una con las coordenadas ya conocidas UTM ZONE 14 N y nombrarla como OS_GEN, ya que sera la capa definitiva

# Campos calculados a Añadir
1. Mediante la tabla de atributos, crear los siguientes 3 campos:

Nombre: GRUPO

tipo: Texto

Nombre: CERRADA_CO

tipo: Texto

Nombre: DIAS_DIF

tipo: long

<img width="857" height="388" alt="image" src="https://github.com/user-attachments/assets/6d9d8e3d-2a43-4e25-8e56-7d1e1d27847a" />

# Calcular campo GRUPO
1. En la calculadora de campos de la tabla de atributos poner las siguientes configuraciones

<img width="569" height="701" alt="image" src="https://github.com/user-attachments/assets/b95de059-e22b-4e85-8d7e-656c0d9116a7" />

2. Este es el codigo para calcular los valores

   ```python
   "Ordenes de Alcantarillado" if !TIPO! == "Alcantarillado" else (
    "Ordenes Fugas de Agua Potable" if !tipo_orden!.startswith("TO831") or !tipo_orden!.startswith("TO832") or !tipo_orden!.startswith("TO833") else (
    "Ordenes de Instalacion de Medidores" if !tipo_orden!.startswith("TO580") or !tipo_orden!.startswith("TO581") or !tipo_orden!.startswith("TO582") or !tipo_orden!.startswith("TO583") or !tipo_orden!.startswith("TO584") else "Sin Grupo Definido"))
   ```
# Calcular campo CERRADA_CO
1. Usar herramiente  CALCULATE FIELD
2. Poner la siguentes configuraciones como se muestra en la siguiente imagen

<img width="967" height="678" alt="image" src="https://github.com/user-attachments/assets/f3cf7f39-fa61-4386-b841-c9c4f8bb27de" />

3. Este es el codigo
### En codeblock
```python
def check_quadrant(latitud, longitud):
    # Definir los límites de los cuadrantes
    cuadrantes = [
        (-100.316973, -100.312949, 25.652214, 25.655862),
        (-100.428287, -100.424259, 25.671859, 25.675509),
        (-100.375847, -100.371821, 25.670696, 25.674344),
        (-100.219353, -100.215330, 25.700751, 25.704396),
        (-100.244167, -100.240144, 25.677858, 25.681503),
        (-100.301215, -100.297188, 25.746247, 25.749894),
        (-100.265576, -100.261549, 25.784094, 25.787739),
        (-100.379165, -100.375137, 25.737090, 25.740739)
    ]
    
    # Verificar si las coordenadas están dentro de uno de los cuadrantes
    for xmin, xmax, ymin, ymax in cuadrantes:
        if xmin <= longitud <= xmax and ymin <= latitud <= ymax:
            return "SI"
    
    # Si no está dentro de ningún cuadrante
    return "NO"
```

### En Expression
```python
check_quadrant(!latitud!, !longitud!)
```
# Calcular campo DIAS_DIF
1. En la calculadora de campos de la tabla de atributos poner las siguientes configuraciones
<img width="570" height="699" alt="image" src="https://github.com/user-attachments/assets/d8470929-bbf7-4a90-9892-d7a25eda3ff5" />

2. Este es el codigo para calcular los valores

   ```vb
   DateDiff("d", [fecha_importacion], [fecha_resolucion])
   ```
   
# Ejemplo de capa OS_GEN con los datos finalmente calculados

<img width="854" height="378" alt="image" src="https://github.com/user-attachments/assets/75a192df-2cbd-499b-9ffc-95ea27ef7a86" />







