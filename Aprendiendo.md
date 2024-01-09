# TIPOS DE DATOS
```python
# Listas (list):Representan secuencias mutables de elementos.

lista_numeros = [1, 2, 3, 4, 5]

# Tuplas (tuple): Representan secuencias inmutables de elementos.

tupla_colores = ("rojo", "verde", "azul")

# Conjuntos (set):Representan colecciones no ordenadas de elementos únicos.

conjunto_frutas = {"manzana", "naranja", "plátano"}

# Diccionarios (dict):Representan colecciones de pares clave-valor.

diccionario_edades = {"Juan": 25, "María": 30, "Carlos": 22}

# NoneType (None):Representa la ausencia de un valor o un valor nulo.

valor_nulo = None
```
**Conversion entre datos**
```python
numero_entero = int(3.14)
float
str
bool #  Utiliza la función bool() para convertir un valor a un booleano. Algunos valores como 0, None, y cadenas vacías son evaluados como False, mientras que otros valores son evaluados como True.
list
tuple
set
dict
    lista_pares = [("a", 1), ("b", 2), ("c", 3)]
    diccionario = dict(lista_pares)
```
**Formatear cadenas**
```python
nombre = "Juan"
edad = 25
print("Hola, %s. Tienes %d años." % (nombre, edad))

nombre = "María"
edad = 30
print("Hola, {}. Tienes {} años.".format(nombre, edad))

nombre = "Carlos"
edad = 22
print(f"Hola, {nombre}. Tienes {edad} años.")

lista_palabras = ["Python", "es", "genial"]
mensaje = " ".join(lista_palabras)
print(mensaje)

```
**Redondeo**
```python
# round(): Esta función redondea un número al entero más cercano
numero = 3.14159
redondeado = round(numero, 2)  # Redondear a 2 decimales
print(redondeado)

# math.floor(): Esta función redondea un número hacia abajo al entero más cercano.
import math

numero = 3.99
redondeado_abajo = math.floor(numero)
print(redondeado_abajo)

# math.ceil(): Esta función redondea un número hacia arriba al entero más cercano.
import math

numero = 2.01
redondeado_arriba = math.ceil(numero)
print(redondeado_arriba)

```
# INDEX, EXTRAER SUB-STRINGS, METODOS Y PROPIEDADES 
**metodo index**

***iterable.index(elemento,inicio,fin)***  
Iterable: La secuencia en la que buscar el elemento (puede ser una lista, tupla o cadena).  
Elemento: El elemento para el cual se busca el índice.  
Inicio (opcional): El índice de inicio para la búsqueda. Si no se proporciona, la búsqueda comienza desde el principio del iterable.  
Fin (opcional): El índice de finalización para la búsqueda. Si no se proporciona, la búsqueda continúa hasta el final del iterable.

```python
frutas = ["manzana", "plátano", "uva", "pera", "uva"]
indice = frutas.index("uva")
print("El índice de 'uva' es:", indice) # Imprime: El índice de 'uva' es 2
```
**substring**
```python
frase = "Python es genial"
# Obtener un substring desde el índice 0 hasta el índice 5 (sin incluir el índice 5)
substring1 = frase[0:5]
print(substring1)  # Output: "Pytho"
```
**metodos de string**  
***metodos de analisis**
```python
# count( ) : retorna el número de veces que se repite un conjunto de caracteres especificado.  
"Hola mundo".count("Hola")  
>> 1  
# find( ) e index( ) retornan la ubicación (comenzando desde el cero) en la que se encuentra el argumento indicado. Difieren en que index lanza ValueError cuando el argumento no es encontrado, mientras find retorna -1.  
"Hola mundo".find("world")  
>> -1  
# rfind( ) y rindex( ).Para buscar un conjunto de caracteres pero desde el final.
"C:/python36/python.exe".rfind("/")
>> 11
# startswith( ) y endswith( ) indican si la cadena en cuestión comienza o termina con el conjunto de caracteres pasados como argumento, y retornan True o False en función de ello.
"Hola mundo".startswith("Hola")
>> True
#isdigit( ): determina si todos los caracteres de la cadena son dígitos, o pueden formar números, incluidos aquellos correspondientes a lenguas orientales.
"abc123".isdigit()
>> False
#isnumeric( ): determina si todos los caracteres de la cadena son números, incluye también caracteres de connotación numérica que no necesariamente son dígitos (por ejemplo, una fracción).
"1234".isnumeric()
>> True
#isdecimal( ): determina si todos los caracteres de la cadena son decimales; esto es, formados por dígitos del 0 al 9.
"1234".isdecimal()
>> True
#isalnum( ): determina si todos los caracteres de la cadena son alfanuméricos.
"abc123".isalnum()
>> True
#isalpha( ): determina si todos los caracteres de la cadena son alfabéticos.
"abc123".isalpha()
>> False
#islower( ): determina si todos los caracteres de la cadena son minúsculas.
"abcdef".islower()
>> True
#isupper( ): determina si todos los caracteres de la cadena son mayúsculas.
"ABCDEF".isupper()
>> True
#isspace( ): determina si todos los caracteres de la cadena son espacios.
"Hola mundo".isspace()
>> False
```
***metodos de transformacion***
En realidad los strings son inmutables; por ende, todos los métodos a continuación no actúan sobre el objeto original sino que retornan uno nuevo.
```python
#capitalize( ) retorna la cadena con su primera letra en mayúscula.
"hola mundo".capitalize()
>> 'Hola mundo'
#encode( ) codifica la cadena con el mapa de caracteres especificado y retorna una instancia del tipo bytes.
"Hola mundo".encode("utf-8")
>> b'Hola mundo'
#replace( ) reemplaza una cadena por otra.
"Hola mundo".replace("mundo","world")
>> 'Hola world'
#lower( ) retorna una copia de la cadena con todas sus letras en minúsculas.
"Hola Mundo!".lower()
>> 'hola mundo!'
#upper( ) retorna una copia de la cadena con todas sus letras en mayúsculas.
#swapcase( ) cambia las mayúsculas por minúsculas y viceversa.
#strip( ), lstrip( ) y rstrip( ) remueven los espacios en blanco que preceden y/o suceden a la cadena.
" Hola mundo! ".strip()
>> 'Hola mundo!'
#Los métodos center( ), ljust( ) y rjust( ) alinean una cadena en el centro, la izquierda o la derecha. Un segundo argumento indica con qué caracter se  
#deben llenar los espacios vacíos (por defecto un espacio en blanco).
"Hola".center(10,"*")
>> '***Hola***'
```
***metodos de separacion***   
```python
#split( ) divide una cadena según un caracter separador (por defecto son espacios en blanco). Un segundo argumento en split( ) indica cuál es el
#máximo de divisiones que puede tener lugar (-1 por defecto para representar una cantidad ilimitada).
"Hola mundo!\nHello world!".split()
>> ['Hola','mundo!','Hello','world!']
#splitlines( ) divide una cadena con cada aparición de un salto de línea.
"Hola mundo!\nHello world!".splitlines()
>> ['Hola mundo!','Hello world!']
#partition( ) retorna una tupla de tres elementos: el bloque de caracteres anterior a la primera ocurrencia del separador, el separador mismo, y el bloque posterior.
"Hola mundo. Hello world!".partition(" ")
>> ('Hola',' ','mundo. Hello world!')
#rpartition( ) opera del mismo modo que el anterior, pero partiendo de derecha a izquierda.
"Hola mundo. Hello world!".rpartition(" ")
>> ('Hola mundo. Hello',' ','world!')
#join( ) debe ser llamado desde una cadena que actúa como separador para unir dentro de una misma cadena resultante los elementos de una lista.
",".join(["C","C++","Python","Java"])
>> 'C, C++, Python, Java'
```
**Propiedades**   
son inmutables: una vez creados, no pueden modificarse sus partes,pero sí pueden reasignarse los valores de las variables a través de métodos de strings

concatenable: es posible unir strings con el símbolo +

multiplicable: es posible concatenar repeticiones de un string con elsímbolo *

multilineales: pueden escribirse en varias líneas al encerrarse entretriples comillas simples (''' ''') o dobles (""" """)

determinar su longitud: a través de la función len(mi_string)

verificar su contenido: a través de las palabras clave in y not in. Elresultado de esta verificación es un booleano (True / False).

**listas**
```python
#indexado: podemos acceder a los elementos de una lista a través de sus índices [inicio:fin:paso]
print(lista_1[1:3])
>> ["C++","Python"]
#cantidad de elementos: a través de la propiedad len( )
print(len(lista_1))
>> 4
#concatenación: sumamos los elementos de varias listas con el símbolo +
#función append( ): agrega un elemento a una lista en el lugar
#función pop( ): elimina un elemento de la lista dado el índice, y devuelve el valor quitado
#función sort( ): ordena los elementos de la lista en el lugar
#función reverse( ): invierte el orden de los elementos en el lugar

```
**sets**
mi_set_a = {1, 2,"tres"}    .  .   .   .       mi_set_b = {3,"tres"}

```python
#add(item) agrega un elemento al set
#clear( ) remueve todos los elementos de un set
#copy( ) retorna una copia del set
mi_set_c = mi_set_a.copy()
print(mi_set_c)
>> {1, 2,"tres"}
#difference(set) retorna el set formado por todos los elementos que únicamente existen en el set A
mi_set_c = mi_set_a.difference(mi_set_b)
print(mi_set_c)
>> {1, 2}
#difference_update(set) remueve de A todos los elementos comunes a A y B
#discard(item) remueve un elemento del set
#intersection(set) retorna el set formado por todos los elementos que existen en A y B simultáneamente.
#intersection_update(set) mantiene únicamente los elementos comunes a A y B
#isdisjoint(set) devuelve True si A y B no tienen elementos en común
#issubset(set) devuelve True si todos los elementos de B están presentes en A
#issuperset(set) devuelve True si A contiene todos los elementos de B
#pop( ) elimina y retorna un elemento al azar del set
#remove(item) elimina un item del set, y arroja error si no existe
#symmetric_difference(set) retorna todos los elementos de A y B, excepto aquellos que son comunes a los dos
mi_set_c = mi_set_b.symmetric_difference(mi_set_a)
print(mi_set_c)
>> {1, 2, 3}
#symmetric_difference_update(set) elimina los elementos comunes a A y B, agregando los que no están presentes en ambos a la vez
mi_set_b.symmetric_difference_update(mi_set_a)
print(mi_set_b)
>> {1, 2, 3}
#union(set) retorna un set resultado de combinar A y B (los datos duplicados se eliminan)
mi_set_c = mi_set_a.union(mi_set_b)
print(mi_set_c)
>> {1, 2, 3,'tres'}
#update(set) inserta en A los elementos de B
mi_set_a.update(mi_set_b)
print(mi_set_a)
>> {1, 2, 3,'tres'}

```
# Funciones   
***argumentos indefinidos**   
Puedes usar *args para permitir que una función acepte un número variable de argumentos posicionales.
```python
def ejemplo(*args):
    for arg in args:
        print(arg)

ejemplo(1, 2, 3, "cuatro", [5, 6]) # imprime: 1 2 3 "cuatro" [5, 6]
```
Puedes usar **kwargs para permitir que una función acepte un número variable de argumentos con nombre (clave-valor).   
```python
def ejemplo_con_nombre(**kwargs):
    for clave, valor in kwargs.items():
        print(f"{clave}: {valor}")

ejemplo_con_nombre(nombre="Juan", edad=25, ciudad="Ejemploville")
>> nombre:Juan edad:25 ciudad:Ejemploville

# para argumentos de diferentes tipos   
def prueba (num1, num2, *args, **kwargs): #seguir ese orden 
    
```
# Manipular archivos
```python
a = open("C:\\xampp\\htdocs\\Python\\01\\prueba.txt")
print(a.read())
a.close()
>>renzo
>>aldave
>>reyes
```
### Para escribir en un archivo desde Python, deberemos elegir con cuidado el parámetro "modo de apertura"    
open(arhivo, modo)   
  
***"r"*** - Read (Lectura) - Predeterminado. Permite leer pero no escribir, y arroja un error si el archivo no existe.   
***"a"*** - Append (Añadir) - Abre el archivo para añadir líneas a continuación de la última que ya exista en el mismo. Crea un archivo en caso de que el mismo no exista.   
***"w"*** - Write (Escritura) - Abre o crea un archivo (si no existe previamente) en modo de escritura, lo que significa que cualquier contenido previo se sobreescribirá.  
***"x"*** - Create (Creación) - Crea un archivo, y arroja un error si el mismo ya existe en el directorio.  
ejemplos  
```python
archivo = open("C:\\xampp\\htdocs\\Python\\01\\prueba.txt","r")
print(archivo.read())
archivo.close()

archivo = open("C:\\xampp\\htdocs\\Python\\01\\prueba.txt","w")
archivo.write('''hola
mundo
aqui
estoy''')
archivo.close()

archivo = open("C:\\xampp\\htdocs\\Python\\01\\prueba.txt","w")
lista = ['hola', 'mundo','aqui','estoy']
for k in lista:
    archivo.writelines(p + '\n')
archivo.close()
```
### Directorios

***os.getcwd():*** obtiene y devuelve el directorio de trabajo actual. Será el mismo en el que corre el programa si no se ha modificado.   
***os.chdir(ruta):*** cambia el directorio de trabajo a la ruta especificada   
***os.makedirs(ruta):*** crea una carpeta, así como todas las carpetas intermedias necesarias de acuerdo a la ruta especificada.   
***os.path.basename(ruta):*** dada una ruta, obtiene el nombre del archivo (nombre de base)   
***os.path.dirname(ruta):*** dada una ruta, obtiene el directorio (carpeta) que almacena el archivo   
***os.path.split(ruta):*** devuelve una tupla que contiene dos elementos: el directorio, y el nombre de base del archivo.   
***rmdir(ruta):*** elimina el directorio indicado en la ruta.   

```python
import os 
ruta = os.getcwd()
print(ruta)
>>C:\xampp\htdocs\Python
--------------------
# Se va trabajar ahora en ese directorio
import os 
ruta = os.chdir('C:\\Users\\HP\\Documents\\23-2')
archivo=open('renzito.txt')
print(archivo.read())
--------------------
# Para crear un nuevo directorio 
import os 
ruta = os.makedirs('C:\\Users\\HP\\Documents\\23-2\\TRABAJOS')
```
### Path lib

***read_text( ):*** lee el contenido del archivo sin necesidad de abrirlo y cerrarlo
***name:*** devuelve el nombre y extensión del archivo
***suffix:*** devuelve la extensión del archivo (sufijo)
***stem:*** devuelve el nombre del archivo sin su extensión (sufijo)
***exists( ):*** verifica si el directorio o archivo al que referencia el objeto Path existe y devuelve un booleano de acuerdo al resultado (True/False)
```python
from pathlib import Path
ruta = Path("C:/xampp/htdocs/Python/01/prueba.txt")

print(ruta.read_text()) #Imprime el contenido del archivo

```
```python
from pathlib import Path,PureWindowsPath
ruta = Path("C:/xampp/htdocs/Python/01/prueba.txt")
ruta_windows= PureWindowsPath(ruta)
print(ruta_windows) # Imprime: C:\xampp\htdocs\Python\01\prueba.txt
```
### Path
```python
from pathlib import Path

base = Path.home()
guia = Path(base,"Barcelona", "Familia",Path("Personas"))
guia2 = guia.with_name("Padre")
print(base) #C:\Users\HP
print(guia) #C:\Users\HP\Barcelona\Familia\Personas
print(guia2) #C:\Users\HP\Barcelona\Familia\Padre
print(guia.parent) #C:\Users\HP\Barcelona\Familia
print(guia.parent.parent) #C:\Users\HP\Barcelona
```
<img src="https://github.com/RenzoAr10/PythonAprender/blob/main/Documentos/Screenshot_27.png" alt="Texto alternativo" width="200">
