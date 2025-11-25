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
1. Mediante la tabla de atributos





