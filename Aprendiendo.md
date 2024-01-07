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
***metodos de analisi**
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
#

#

#








```


