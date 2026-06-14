# Clase 02 — Control de Flujo y Estructuras de Datos


> 🤔 **Para pensar antes de leer:** Tienes una planilla con 10.000 filas de ventas. Alguien te pide que encuentres todas las transacciones mayores a $100.000 y que cuentes cuántas corresponden a clientes nuevos. ¿Cómo lo harías? ¿Y si mañana son 100.000 filas?


En la clase anterior aprendiste a guardar información en variables. Puedes decirle a Python que el nombre de un cliente es `"Ana"`, que su edad es `29`, que su compra fue de `84500.0`. Eso es útil, pero es pasivo. El programa guarda datos y no hace nada más. Lo que hace interesante a un programa es que puede tomar decisiones y repetir operaciones. Puede revisar miles de registros en segundos, aplicar una regla a cada uno, y darte solo lo que te importa. Para eso existen dos herramientas fundamentales: el control de flujo y las estructuras de datos. Esta clase cubre ambas.

### Tomar decisiones con `if`

Cuando analizas datos, constantemente aplicas reglas: si el monto supera cierto límite, es una venta importante. Si el stock llega a cero, hay que reordenar. Si la temperatura del servidor sube demasiado, hay que generar una alerta. En tu cabeza eso es automático. En Python, tienes que escribirlo explícitamente.

La herramienta para eso es el condicional `if`. Le decís a Python: evalúa esta condición, y si es verdadera, ejecuta este bloque de código.

```python
temperatura = 87.3

if temperatura > 80:
    print("Estado: crítico")
```

Python evalúa `temperatura > 80`. El resultado es `True` o `False`. Si es `True`, ejecuta lo que está adentro del bloque. Si es `False`, lo ignora y sigue. El detalle importante es la indentación. Los cuatro espacios antes del `print` no son decorativos: le indican a Python que esa línea pertenece al bloque del `if`. Si los sacas, el código hace algo completamente distinto, o directamente falla.

La mayoría de las veces no tienes solo dos casos posibles, sino varios. Para eso existe `elif` (abreviatura de "else if") y `else`:

```python
temperatura = 87.3

if temperatura <= 60:
    estado = "normal"
elif temperatura <= 80:
    estado = "advertencia"
else:
    estado = "crítico"

print(f"Temperatura: {temperatura}°C — Estado: {estado}")
```

Python evalúa las condiciones en orden. En cuanto una es `True`, ejecuta ese bloque y se saltea el resto. El `else` actúa como un caso por defecto: se ejecuta solo si ninguna condición anterior fue verdadera. También puedes combinar condiciones con `and`, `or` y `not`. `and` requiere que ambas sean verdaderas. `or` requiere que al menos una lo sea. `not` invierte el resultado.

```python
monto = 250000
cliente_nuevo = True

if monto > 100000 and cliente_nuevo:
    categoria = "cliente nuevo de alto valor"
elif monto > 100000 and not cliente_nuevo:
    categoria = "cliente recurrente de alto valor"
else:
    categoria = "cliente estándar"
```

Leídos en voz alta, `and`, `or` y `not` se comportan exactamente como sus equivalentes en español. Eso hace que las condiciones compuestas sean bastante legibles una vez que te acostumbras a la sintaxis.

### Repetir operaciones con bucles

Un condicional resuelve el problema de tomar decisiones. Pero hay otro problema que aparece todo el tiempo en data analysis: tienes que hacer lo mismo con muchos datos. Si tuvieras que calcular el precio con IVA de 500 productos usando solo lo que sabes hasta ahora, escribirías 500 líneas casi idénticas. Eso no es programar, es copiar y pegar. Los bucles existen para eso: ejecutar el mismo bloque de código muchas veces, cambiando solo lo que tiene que cambiar.

#### El bucle `for`

Usas `for` cuando tienes una colección de elementos y quieres hacer algo con cada uno. El bucle los recorre de principio a fin, uno por vez.

```python
precios = [15000, 32000, 8500, 71000, 12300]

for precio in precios:
    precio_con_iva = precio * 1.21
    print(f"${precio} → ${precio_con_iva:.2f}")
```

Leído en español: *para cada `precio` que haya en `precios`, calcula el precio con IVA e imprimilo*. La variable `precio` la crea Python automáticamente en cada iteración y le asigna el valor del elemento actual. Cuando se acaban los elementos, el bucle termina.

Cuando no tienes una colección sino que simplemente quieres repetir algo N veces, usas `range()`:

```python
for i in range(5):
    print(f"Iteración {i}")
# Imprime: 0, 1, 2, 3, 4
```

`range(5)` genera los números del 0 al 4. `range(1, 6)` genera del 1 al 5. `range(0, 10, 2)` genera 0, 2, 4, 6, 8 — el tercer parámetro es el paso entre cada número.

#### El bucle `while`

`for` recorre una colección y termina. `while` funciona diferente: repite un bloque de código mientras una condición sea verdadera, sin importar cuántas veces sea necesario.

```python
intentos = 0

while intentos < 3:
    clave = input("Ingresá tu clave: ")
    if clave == "python2024":
        print("Acceso concedido")
        break
    intentos += 1
    print(f"Clave incorrecta. Intentos restantes: {3 - intentos}")
```

El riesgo del `while` es el bucle infinito: si la condición nunca se vuelve `False`, el programa corre para siempre. Por eso siempre tiene que haber algo dentro del bucle que eventualmente cambie la condición. En este ejemplo, `intentos += 1` garantiza que el bucle va a terminar.

`break` es una instrucción que sale del bucle inmediatamente, sin esperar a que la condición sea falsa. Su opuesto, `continue`, salta al inicio de la siguiente iteración sin terminar la actual.

#### Acumular resultados

Un patrón muy común es recorrer datos y construir un resultado a medida que avanzas:

```python
ventas = [45000, 32000, 67500, 28000, 91000]

total = 0
ventas_altas = 0

for venta en ventas:
    total += venta
    if venta > 50000:
        ventas_altas += 1

promedio = total / len(ventas)

print(f"Total: ${total:,}")
print(f"Promedio: ${promedio:,.2f}")
print(f"Ventas sobre $50.000: {ventas_altas}")
```

`total` empieza en `0` y se construye iteración por iteración. Ese patrón — inicializar un acumulador antes del bucle y modificarlo adentro — es uno de los más usados en programación. Lo vas a reconocer en muchos contextos distintos.

### Guardar múltiples valores con listas

Hasta ahora cada variable guardaba un solo valor. Una lista te permite guardar múltiples valores en una sola variable, en un orden específico. Ese orden importa: el primer elemento está en la posición 0, el segundo en la posición 1, y así.

```python
temperaturas = [62.0, 75.0, 91.0, 58.5, 83.2]

print(temperaturas[0])     # 62.0
print(temperaturas[-1])    # 83.2 — el índice -1 siempre es el último
print(temperaturas[1:3])   # [75.0, 91.0] — del índice 1 al 2
```

Los índices negativos son una característica de Python: `-1` es el último elemento, `-2` el antepenúltimo. Es muy útil cuando no sabes exactamente cuántos elementos tiene la lista.

Las listas son mutables, lo que significa que puedes modificarlas después de crearlas:

```python
productos = ["notebook", "mouse", "teclado"]

productos.append("monitor")         # agrega al final
productos.insert(1, "auriculares")  # inserta en la posición 1
productos.remove("mouse")           # elimina el primer "mouse" que encuentre
ultimo = productos.pop()            # elimina y devuelve el último elemento
```

Para operaciones numéricas sobre listas, Python tiene funciones integradas:

```python
numeros = [5, 2, 8, 1, 9, 3, 7]

print(min(numeros))      # 1
print(max(numeros))      # 9
print(sum(numeros))      # 35
print(sorted(numeros))   # [1, 2, 3, 5, 7, 8, 9] — devuelve una lista nueva
```

#### List comprehensions

Una list comprehension es una forma compacta de crear una lista aplicando una operación a cada elemento de otra. Es una de las características más expresivas de Python, y vale la pena aprenderla desde el principio:

```python
precios = [15000, 32000, 8500, 71000, 12300]

# Sin list comprehension:
precios_con_iva = []
for p in precios:
    precios_con_iva.append(p * 1.21)

# Con list comprehension — hace exactamente lo mismo:
precios_con_iva = [p * 1.21 for p in precios]

# Con filtro:
precios_altos = [p for p in precios if p > 20000]
```

Se lee casi en español: *"dame `p * 1.21` para cada `p` en `precios`"*. Con el tiempo esta sintaxis se vuelve más natural que la versión con el bucle explícito.

### Organizar datos con diccionarios

En una lista accedes a los elementos por su posición. Eso funciona cuando el orden importa, pero no siempre es el caso. Los datos de un cliente — nombre, edad, email, país — no tienen un orden natural. Es más claro acceder a ellos por nombre que por índice.

Un diccionario te permite hacer eso: guardar pares de clave-valor, donde cada valor tiene un nombre descriptivo.

```python
cliente = {
    "nombre": "Ana Torres",
    "edad": 29,
    "email": "ana@ejemplo.com",
    "pais": "Argentina",
    "es_premium": True,
    "total_compras": 84500.0
}

print(cliente["nombre"])          # Ana Torres
print(cliente["total_compras"])   # 84500.0
```

Para acceder a un valor, usas la clave entre corchetes. Si la clave no existe, Python lanza un `KeyError` que interrumpe el programa. Para evitar eso, usas `.get()`:

```python
print(cliente.get("telefono", "no registrado"))   # no registrado
```

`.get()` recibe la clave y un valor por defecto. Si la clave no existe, devuelve el valor por defecto en lugar de fallar. En data analysis, donde los campos faltantes son comunes, esta distinción importa mucho.

Puedes modificar un diccionario en cualquier momento:

```python
cliente["telefono"] = "+54 11 1234-5678"   # agrega o actualiza
del cliente["email"]                        # elimina la clave
```

Para recorrer un diccionario, lo más común es iterar sobre sus pares clave-valor con `.items()`:

```python
for clave, valor in cliente.items():
    print(f"{clave}: {valor}")
```

#### Lista de diccionarios

En la práctica, casi nunca trabajas con un solo diccionario. Trabajas con una lista de diccionarios, donde cada diccionario representa un registro:

```python
ventas = [
    {"fecha": "2024-11-01", "vendedor": "Carlos", "monto": 45000, "pais": "AR"},
    {"fecha": "2024-11-01", "vendedor": "Laura",  "monto": 82000, "pais": "MX"},
    {"fecha": "2024-11-02", "vendedor": "Carlos", "monto": 31000, "pais": "AR"},
    {"fecha": "2024-11-02", "vendedor": "Laura",  "monto": 95000, "pais": "MX"},
]

total = sum(v["monto"] for v in ventas)
ventas_ar = [v for v in ventas if v["pais"] == "AR"]

print(f"Total: ${total:,}")
print(f"Ventas en AR: {len(ventas_ar)}")
```

Esta estructura — lista de diccionarios — es exactamente lo que pandas convierte en un DataFrame. Cuando en unidades siguientes escribas `pd.DataFrame(ventas)`, pandas va a usar las claves como nombres de columnas y cada diccionario como una fila. Ya sabes cómo funciona por dentro.

### Todo junto

Antes de usar pandas puedes hacer análisis reales combinando lo que aprendiste hoy. Este ejemplo recorre un dataset, clasifica, agrupa y genera un reporte:

```python
registros = [
    {"producto": "Notebook",  "categoria": "tech",    "precio": 320000, "stock": 5},
    {"producto": "Mouse",     "categoria": "tech",    "precio": 8500,   "stock": 42},
    {"producto": "Escritorio","categoria": "muebles", "precio": 85000,  "stock": 3},
    {"producto": "Silla",     "categoria": "muebles", "precio": 45000,  "stock": 8},
    {"producto": "Monitor",   "categoria": "tech",    "precio": 210000, "stock": 0},
    {"producto": "Lámpara",   "categoria": "muebles", "precio": 12000,  "stock": 15},
]

totales = {}
sin_stock = []

for r in registros:
    cat = r["categoria"]

    if cat not in totales:
        totales[cat] = {"cantidad": 0, "valor": 0}

    totales[cat]["cantidad"] += 1
    totales[cat]["valor"] += r["precio"] * r["stock"]

    if r["stock"] == 0:
        sin_stock.append(r["producto"])

print("REPORTE DE INVENTARIO")
print("─" * 36)

for categoria, datos in totales.items():
    print(f"\n{categoria.upper()}")
    print(f"  Productos:   {datos['cantidad']}")
    print(f"  Valor total: ${datos['valor']:,}")

if sin_stock:
    print(f"\nSin stock: {', '.join(sin_stock)}")
```

No hay nada en ese código que no hayas visto en esta clase. Y hace algo que en Excel requeriría tablas dinámicas, filtros manuales y bastante paciencia.


## Resumen

| Concepto | Para qué sirve |
|----------|----------------|
| `if / elif / else` | Ejecutar código distinto según una condición |
| `for` | Recorrer una colección de principio a fin |
| `while` | Repetir mientras una condición sea verdadera |
| `break` | Salir de un bucle antes de que termine |
| `continue` | Saltar a la siguiente iteración |
| Lista `[]` | Colección ordenada, acceso por índice numérico |
| Diccionario `{}` | Colección de pares clave-valor, acceso por nombre |
| `.append()` | Agregar un elemento al final de una lista |
| `.get(clave, default)` | Acceder a un diccionario sin riesgo de `KeyError` |
| `.items()` | Iterar sobre claves y valores de un diccionario |
| List comprehension | Crear listas de forma compacta con una expresión |


## Recursos adicionales

- [Python Docs — Control Flow](https://docs.python.org/3/tutorial/controlflow.html)
- [Python Docs — Data Structures](https://docs.python.org/3/tutorial/datastructures.html)
- [Real Python — Python for Loops](https://realpython.com/python-for-loop/)
- [Real Python — Dictionaries in Python](https://realpython.com/python-dicts/)

---

## Práctica

→ [Ver ejercicios](./practica/ejercicios.md)

---

*← [Clase 01 — Introducción al Data Analysis](../clase-01/README.md) · [Módulo 1](../README.md) · Clase 03 — Funciones →*