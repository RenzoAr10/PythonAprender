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

***read_text( ):*** lee el contenido del archivo sin necesidad de abrirlo y cerrarlo.   
***name:*** devuelve el nombre y extensión del archivo.   
***suffix:*** devuelve la extensión del archivo (sufijo).  
***stem:*** devuelve el nombre del archivo sin su extensión (sufijo).  
***exists( ):*** verifica si el directorio o archivo al que referencia el objeto Path existe y devuelve un booleano de acuerdo al resultado (True/False).  
***write(contenido):*** para agregar contenido al archivo.   

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
#-------------------------------------------------------------------------------------
#Se crea la carpeta 
ruta_carpeta = Path("C:\\xampp\\htdocs\\Python\\Mis_modulos")
ruta_carpeta.mkdir()

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
<img src="https://github.com/RenzoAr10/PythonAprender/blob/main/Documentos/Screenshot_27.png" alt="Texto alternativo" width="600">

```python
from pathlib import Path

guia = Path(Path.home(),"Europa")
for txt in Path(guia).glob("*.txt"):
    print(txt)
# C:\Users\HP\Europa\Consejos.txt
# C:\Users\HP\Europa\Normativas.txt

for txt in Path(guia).glob("**/*.txt"):
    print(txt)
# C:\Users\HP\Europa\Consejos.txt
# C:\Users\HP\Europa\Normativas.txt
# C:\Users\HP\Europa\España\Barcelona\LaPedrera.txt
# C:\Users\HP\Europa\España\Barcelona\SagradaFamilia.txt
# C:\Users\HP\Europa\España\Madrid\MuseoDelPrado.txt
# C:\Users\HP\Europa\Francia\Paris\TorreEiffel.txt

guia2=Path("Europa","España", "Barcelona","SagradaFamilia.txt")
en_europa=guia2.relative_to(Path("Europa"))
en_espania=guia2.relative_to(Path("Europa","España"))
print(en_europa) # España\Barcelona\SagradaFamilia.txt
print(en_espania) # Barcelona\SagradaFamilia.txt
```
# Programacion Orientada de Objetos
Los atributos son variables que pertenecen a la clase. Existen atributos de clase (compartidos por todas las instancias de la clase), y de instancia (que son distintos en cada instancia de la clase).  

Todas las clases tienen una función que se ejecuta al instanciarla, llamada _ _init__(), y que se utiliza para asignar valores a las propiedades del objeto que está siendo creado.    
***self:*** representa a la instancia del objeto que se va a crear. 

***Tipos de metodos***

<img src="https://github.com/RenzoAr10/PythonAprender/blob/main/Documentos/Screenshot_28.png" alt="Texto alternativo" width="600">

```python
class pajaro :

    alas=True

    def __init__(self,mi_color,especie):
        self.color=mi_color 
        self.especie=especie

    def piar(self):
        print(f"pio,mi color es {self.color}")

    def volar(self,metros):
        print(f"El pajaro ha volado {metros} metros")
        self.piar()

    def pintarNegro(self):
        self.color="Negro"
        print(f"El pajaro es de color {self.color}")

    @classmethod
    def ponerHuevos (cls,cantidad):
        print(f"Puso {cantidad} huevos")
        cls.alas=False

    @staticmethod
    def mirar():
        print("El pajaro mira")

mi_pajaro=pajaro("negro","condor") 

print(mi_pajaro.color) # negro
print(pajaro.alas) # True
print(mi_pajaro.alas) # True

mi_pajaro.piar() # pio,mi color es negro
mi_pajaro.volar(50) # El pajaro ha volado 50 metros
#-----------------------------------------------------------------------------#
piolin = pajaro("Amarillo","Avestruz")
piolin.pintarNegro() #El pajaro es de color Negro
piolin.volar(70) #El pajaro ha volado 70 metros
                 #pio,mi color es Negro
#----------------------------------------------------------------------
pajaro.ponerHuevos(3) # Puso 3 huevos
#----------------------------------------------------
pajaro.mirar() #El pajaro mira
```
### Herencia

La herencia es el proceso mediante el cual una clase puede tomar métodos y atributos de una clase superior, evitandorepetición del código cuando varias clases tienen atributos o métodos en común.
```python
class Personaje:
    def __init__(self, nombre, arma):
        self.nombre = nombre
        self.arma = arma
        
class Mago(Personaje):
    pass

hechicero = Mago("Merlín","caldero")
```
***Herencia multiple***
```python
class animal:
    def __init__(self,edad,color):
        self.edad = edad
        self.color = color

    def nacer(self):
        print("el animal ha nacido")

    def hablar (self):
        print("Este animal emite un sonido")

class oviparo:
    def poner(self):
        print("Este animal pone huevos")


class pajaro (animal,oviparo):
    def __init__(self,edad,color,altura_vuelo):
        self.edad = edad
        self.color = color 
        # super().__init__(edad,color)  

        self.altura_vuelo=altura_vuelo

    def hablar(self):
        print("Pio")
    def volar(self,metros):
        print(f"El pajaro vuela {metros} metros")

piolin = pajaro(3,"Rojo",100)
piolin.volar(150) # El pajaro vuela 150 metros
piolin.hablar() # Pio
piolin.nacer() # el animal ha nacido
print(pajaro.__mro__) #(<class '__main__.pajaro'>, <class '__main__.animal'>, <class '__main__.oviparo'>, <class 'object'>)
```
### Polimorfismo
El polimorfismo es el pilar de la POO mediante el cual un mismo método puede comportarse de diferentes maneras según el objeto sobre el cual esté actuando, en función de cómo dicho método ha sido creado para la clase en particular.  

```python
class Perro:
    def hablar(self):
        print("Guau!")

class Gato:
    def hablar(self):
        print("Miau!")

hachiko = Perro()
garfield = Gato()

for animal in [hachiko, garfield]:
    animal.hablar()
#Guau!
#Miau!
    
def animal_habla(animal):
    animal.hablar()

animal_habla(hachiko) # Guau!
```
### Metodos especiales 
Puedes encontrarlos con el nombre de métodos mágicos o dunder methods (del inglés: dunder = double underscore, o doble guión bajo).  
```python
class Libro:

    def __init__(self, autor, titulo, cant_paginas):

        self.autor = autor
        self.titulo = titulo
        self.cant_paginas = cant_paginas

    def __str__(self):

        return f'Título: "{self.titulo}", escrito por {self.autor}'

    def __len__(self):
        return self.cant_paginas
    

libro1 = Libro("Stephen King","It", 1032)

print(str(libro1)) # Título: "It", escrito por Stephen King
print(len(libro1)) # 1032

```
# Paquetes   

En "simbolo del sistema"    
Si ya conoces el nombre del módulo que quieres instalar, puedes obtenerlo de PyPi directamente desde la consola:  
**pip install < modulo >**  
También podrás actualizar los módulos que ya tengas instalados añadiendo --upgrade luego del nombre del paquete:  
**pip install < modulo > --upgrade**

### Modulos y paquetes

<img src="https://github.com/RenzoAr10/PythonAprender/blob/main/Documentos/Screenshot_29.png" alt="Texto alternativo" width="500">

```python
from paquete_Fede import suma_y_resta

suma_y_resta.resta(18,5) #13
suma_y_resta.suma(15,8) #23

from paquete_Fede.subpaquete import hablar
hablar.hola() # Hola bb
```
### Manejo de errores
```python
try:
    # Código que podría generar una excepción
    resultado = 10 / 0  # Intentando dividir por cero
except ZeroDivisionError as e:
    # Manejo específico para la excepción ZeroDivisionError
    print(f"Error: {e}")
except Exception as e:
    # Manejo de otras excepciones
    print(f"Otro error: {e}")
else:
    # Código que se ejecuta si no se produce ninguna excepción en el bloque try
    print("Operación exitosa.")
finally:
    # Código que se ejecuta siempre, independientemente de si se produce una excepción o no
    print("Finalizando...")
```
***pylint***   

Pylint es un verificador de código, errores y calidad para Python, siguiendo el estilo recomendado por PEP 8, la guía de estilo de Python. Es de gran utilidad en el trabajo en equipo. Python TOTAL

***pip install pylint***   
C:\... ruta> pylint modulo1.py -r y

***unittest***

<img src="https://github.com/RenzoAr10/PythonAprender/blob/main/Documentos/Screenshot_30.png" alt="Texto alternativo" width="500">

### Decoradores

Los decoradores son patrones de diseño en Python utilizados para dar nueva funcionalidad a objetos (funciones), modificando su comportamiento sin alterar su estructura: son funciones que modifican funciones.

```python
def decorar_palabra(funcion):

    def otra_funcion(palabra):
        print("hola")
        funcion(palabra)
        print("adios")
    return otra_funcion

def mayuscula(texto):
    print(texto.upper())

def minuscula(texto):
    print(texto.lower())

mayuscula_decorada= decorar_palabra(mayuscula)
mayuscula_decorada("feDe")
# hola
# FEDE
# adios
```
### Generadores

Los generadores son tipos especiales de funciones que devuelven un iterador que no almacena su contenido completo en memoria, sino que "demora" la ejecución de una expresión hasta que su valor se solicita.

```python
def mi_funcion ():
    lista =[]
    for i in range(1,5):
        lista.append(i*10)
    return lista

def mi_generador ():
    for i in range(1,5):
        yield i * 10

print(mi_funcion()) #[10, 20, 30, 40]
print(mi_generador) #<function mi_generador at 0x000001C499F1D260>

g= mi_generador() 

print(next(g)) #10
print(next(g)) #20

#-----------------------------------------------------------------
        
def otro_generador():
    x=1
    yield x
    x+=1
    yield x
    x+=1
    yield x

p=otro_generador()
print(next(p)) # 1
print(next(p)) # 2
print(next(p)) # 3
```
# Collections 

### collections
***Counters (Contadores):*** Es una subclase del diccionario, usado para contar las repeticiones de cada elemento en un iterable, en forma de diccionario.  
***DefaultDict:*** Es una subclase del diccionario, usado para proporcionar valores por defecto para las claves que no existan, sin generar un mensaje de error. El valor por defecto puede ser un tipo de dato (int, list, etc.) o una función lambda que proporcione dicho valor directamente (lambda:"mi valor").  
***NamedTuple:*** devuelve una tupla donde las posiciones de sus elementos tienen un nombre, además de un número de índice como las tuplas tradicionales.  

"mi valor").
```python
from collections import Counter

numeros = [1,1,1,2,2,3,3,3,3,5,6,6,7]
frase = "Al pan pan y al vino vino"
print(Counter(numeros)) # Counter({3: 4, 1: 3, 2: 2, 6: 2, 5: 1, 7: 1})
print(Counter("missisipi")) # Counter({'i': 4, 's': 3, 'm': 1, 'p': 1})
print(Counter(frase.split())) # Counter({'pan': 2, 'vino': 2, 'Al': 1, 'y': 1, 'al': 1})

serie = Counter([1,1,1,2,2,3,3,3,3,5,6,6,7])
print(serie.most_common()) # [(3, 4), (1, 3), (2, 2), (6, 2), (5, 1), (7, 1)]
print(serie.most_common(1)) # [(3, 4)]
print(list(serie)) # [1, 2, 3, 5, 6, 7]

from collections import defaultdict

mi_dic = {'uno':'verde', 'dos':'azul','tres':'rojo'}
# sale error si se coloca 'cuatro'
mi_dic = defaultdict(lambda: 'nada')
print(mi_dic['cuatro']) # nada
print(mi_dic) # defaultdict(<function <lambda> at 0x000002727E3004A0>, {'cuatro': 'nada'})

from collections import namedtuple

tupla =  namedtuple('Persona',['nombre','altura','peso'])
persona1=tupla('Ariel',1.72,72)
print(persona1.peso) #72
print(persona1[1]) #1.72
```
### modulos OS y Shutil

El módulo Shutil ofrece funcionalidades de alto nivel sobrearchivos, tales como copiar, crear, eliminar y mover entre directorios. También mencionaremos métodos del módulo os que cumplen objetivos similares.    
***shutil.move(archivo, directorio) :*** mueve un archivo desde el directorio actual hacia aquel que se especifica en el segundo parámetro.  
***os.unlink(directorio) :*** elimina un archivo del directorio indicado.   
***os.rmdir(directorio) :*** elimina una carpeta vacía.    
***shutil.rmtree(directorio) :*** elimina una carpeta indicada en el directorio, incluyendo todas sus ramificaciones (subcarpetas y archivos), de manera definitiva y sin pasar porla papelera de reciclaje.   
***send2trash.send2trash(archivo) :*** envía un archivo a la papelera de reciclaje (es necesario instalar el módulo desde y luego importarlo).  
***os.walk(directorio) :*** recorre el directorio indicado, y devuelve los nombres de carpetas, subcarpetas y archivos dentro de ellas en forma de tupla, a través de un generador.  
***archivo[0].startswith("ANTES")***: compara si el archivo comienza con "ANTES"

```python
import os, shutil, send2trash
print(os.getcwd())

shutil.move('C:\\xampp\\htdocs\\Python\\cursoPython\\ejercicio.py','C:\\Users\\HP\\Desktop') 

#pip install send2trash
send2trash.send2trash('C:\\Users\\HP\\Desktop\\ejercicio.py') #archivo a papelera

```
```python
import os, shutil, send2trash

ruta='C:\\Users\\HP\\Documents\\23-2'

for carpeta,subcarpeta, archivo in os.walk(ruta):
    print(f"En la carpeta {carpeta}")
    print("Las subcarpetas son: ")
    for sub in subcarpeta:
        print(f"\t{sub}")
    print("Los archivos son: ")
    for arch in archivo:
        print(f"\t{arch}")
    print("\n")

```
### Modulo datetime 

```python
import datetime
miHora = datetime.time(17,35)
print(miHora) # 17:35:00
print(miHora.hour) # 17 

miDia = datetime.date(2025,10,17)
print(miDia) # 2025-10-17
print(miDia.ctime()) # Fri Oct 17 00:00:00 2025

print(miDia.today()) #Fecha actual 2024-01-13

from datetime import datetime
miFecha = datetime(2025,5,17,22,10,15,2000)
print(miFecha) # 2025-05-17 22:10:15.002000
miFecha = miFecha.replace(month=11)
print(miFecha) # 2025-11-17 22:10:15.002000

despierta = datetime(2022,10,16,7,30)
duerme = datetime(2022,10,16,23,50)
vigilia = duerme - despierta
print(vigilia) # 16:20:00


from datetime import date
nacimiento = date ( 1997,10,17)
defuncion = date (2029,12,16)
vida = defuncion-nacimiento
print(vida) # 11748 days, 0:00:00

```
### Time 
```python
import time 
inicio = time.time()
[código]
final = time.time()
duracion = final - inicio # mide el tiempo, no es exacto para valores pequeños 
#--------------------------------------------------------------------------
import timeit

def  prueba_while ():
    pass

declaracion = """prueba_while (10)"""
mi_setup ="""
def  prueba_while ():
    pass
"""

tiempo = timeit.timeit(declaracion,mi_setup,number=1000000)
```
### Modulo Math

Funciones trigonométricas (sin(), cos(), tan()).   
Funciones exponenciales y logarítmicas (exp(), log(), log10()).   
Funciones de redondeo (ceil(), floor()).  
Constantes matemáticas como pi (math.pi).  
```python
import math

# Calcular el seno de 30 grados
seno_30 = math.sin(math.radians(30))
print(seno_30)

# Calcular el logaritmo natural de 2
logaritmo_2 = math.log(2)
print(logaritmo_2)

# Imprimir el valor de pi
print(math.pi)

```
### Modulo Numpy

Es una biblioteca externa de Python que proporciona soporte para arrays multidimensionales y funciones matemáticas avanzadas. Es especialmente útil para operaciones numéricas y computacionales, y es ampliamente utilizado en el ámbito de la ciencia de datos y la computación científica.
```python
import numpy as np

# Crear un array de una dimensión
arr = np.array([1, 2, 3, 4, 5])

# Calcular la media y la desviación estándar del array
media = np.mean(arr)
desviacion_estandar = np.std(arr)

print("Media:", media)
print("Desviación estándar:", desviacion_estandar)

```
### Modulo RE

<img src="https://github.com/RenzoAr10/PythonAprender/blob/main/Documentos/Screenshot_31.png" alt="Texto alternativo" width="800">

```python
import re

texto = "La expresión regular en Python es poderosa.Python"
patron = "Python"

resultado = re.search(patron, texto)

print("Encontrado:", resultado.group()) # Encontrado: Python
print(resultado.span()) # (24, 30)

resultado2=re.findall(patron,texto)
print(resultado2) # ['Python', 'Python']

#-----------------------------------------------------

texto2= 'llama al 978-789-7458 ya mismo'
patron2=r'\d\d\d-\d\d\d-\d\d\d\d'
resultado3= re.search(patron2,texto2)
print(resultado3) # <re.Match object; span=(9, 21), match='978-789-7458'

#------------------------------------------------------
tex = "No atendemos los martes en la tarde"
buscar = re.search(r'lunes|martes',tex)
print(buscar) # <re.Match object; span=(17, 23), match='martes'>

buscar = re.search(r'....demos...',tex)
print(buscar) # <re.Match object; span=(3, 15), match='atendemos lo'>

tex2 = "4No atendemos los martes en la tarde8"

# ^: Ancla al principio de la cadena.
buscar = re.search(r'^\D', tex2)
print(buscar)  # None

# $: Ancla al final de la cadena.
buscar = re.search(r'\D$', tex2)
print(buscar)  # None

buscar = re.findall(r'[^\s]', tex)  # Excluye los caracteres que no son espacios en blanco
print(buscar)  # ['N', 'o', 'a', 't', 'e', 'n', 'd', 'e', 'm', 'o', ...]

buscar = re.findall(r'[^\s]+', tex)  # Excluye los caracteres que no son espacios en blanco, pero en grupos
print(buscar)  # ['No', 'atendemos', 'los', 'martes', 'en', 'la', 'tarde']
print(' '.join(buscar))  # No atendemos los martes en la tarde

```
### Comprimir y descomprimir archivos 

```python

import zipfile
# Para pasar archivos a la carpeta comprimida
mi_zip = zipfile.ZipFile('carpeta_comprimida.zip','w')

mi_zip.write('texto_A.txt')
mi_zip.write('texto_B.txt')

mi_zip.close()
-----------------------------------------

# Para extraer los archivos de la carpeta
zip_abierto = zipfile.ZipFile('carpeta_comprimida.zip','r')
zip_abierto.extractall()

----------------------------------------------
import shutil

carpeta_origen = 'C:\\ruta de la carpeta para usar sus archivos a comprimir'
archivo_destino = 'Nombre de la carpeta comprimida'
shutil.make_archive(archivo_destino,'zip',carpeta_origen)
------------------------------------------------
import shutil
# descomprimir de una a otra carpeta 
shutil.unpack_archive('la carpeta comprimida.zip','Nombre de la extraccion','zip')

```
# Crear pantalla 

pip install pygame

### crear pantalla

```python
import pygame 

pygame.init()

pantalla = pygame.display.set_mode((800,600))

se_ejecuta = True
while se_ejecuta:
    for evento in pygame.event.get():
        if evento.type==pygame.QUIT:
            se_ejecuta = False

pygame.quit()
```
### Cambiar titulo, icono y color

Para iconos: https://www.flaticon.es/   
Para el color: https://colorspire.com/   

# Web Scraping
REGLAS: se debe tener permisos para acceder a la informacion de la pagina, sino se puede bloquear tu IP.    
pip install beautifulsoup4   
pip install lxml   
pip install requests   

<img src="https://github.com/RenzoAr10/PythonAprender/blob/main/Documentos/Screenshot_32.png" alt="Texto alternativo" width="600">

### Extraer elementos de una clase
```python
import bs4, requests 

resultado = requests.get('https://escueladirecta-blog.blogspot.com/2023/05/configurando-la-impresion-perfecta-de.html')
sopa = bs4.BeautifulSoup(resultado.text,'lxml')

print(sopa.select('title')) # [<title>Configurando la Impresión Perfecta de un Libro de Excel</title>] 
print(len(sopa.select('p'))) # 37, es una lista 
print(sopa.select('title')[0].getText()) # Configurando la Impresión Perfecta de un Libro de Excel

#Extraer elementos de una clase 
columna_lateral = sopa.select('.slide p')
print(columna_lateral)

for p in columna_lateral:
    print(p.getText())
    print("\n")
```
### Extraer imagenes 
```python
import bs4, requests 

resultado = requests.get('https://escueladirecta.com/courses/')
sopa = bs4.BeautifulSoup(resultado.text,'lxml')

# imagenes = sopa.select('img') # Se tiene que la clase que se busca #course-box-image"
imagenes = sopa.select('.course-box-image')[0]['src'] # Se tiene la solo la url 
print(imagenes)

imagen_curso_1 = requests.get(imagenes)

f = open('mi_imagen.jpg','wb')
f.write(imagen_curso_1.content)
# Se crea en la carpeta "C:\xampp\htdocs\Python\mi_imagen.jpg"
```
### Explorar multiples paginas, identificar condiciones de extraccion y extrael titulo del libro    

https://toscrape.com/

```python
import bs4, requests 

url_base = 'https://books.toscrape.com/catalogue/page-{}.html'

resultado = requests.get(url_base.format('1'))
sopa = bs4.BeautifulSoup(resultado.text, 'lxml')

# print(sopa.select('.product_pod')) #Una lista de 20 elementos
print(len(sopa.select('.product_pod'))) # 20 

# Extraer el titulo del libro
libros = sopa.select('.product_pod') #toda la info de los productos en una lista
ejemplo = libros[0].select('a')[1]['title']
print(ejemplo) # A Light in the Attic

```



