El módulo doctest busca pedazos de texto que lucen como sesiones interactivas de Python, y entonces ejecuta esas sesiones para verificar que funcionen exactamente como son mostradas. Hay varias maneras de usar doctest:

Para revisar que los docstrings de un módulo están al día al verificar que todos los ejemplos interactivos todavía trabajan como está documentado.

Para realizar pruebas de regresión al verificar que los ejemplos interactivos de un archivo de pruebas o un objeto de pruebas trabaje como sea esperado.

Para escribir documentación de tutorial para un paquete, generosamente ilustrado con ejemplos de entrada-salida. Dependiendo si los ejemplos o el texto expositivo son enfatizados, tiene el sabor de «pruebas literarias» o «documentación ejecutable».

Aquí hay un módulo de ejemplo completo pero pequeño:

"""
This is the "example" module.

The example module supplies one function, factorial().  For example,

>>> factorial(5)
120
"""

def factorial(n):
    """Return the factorial of n, an exact integer >= 0.

    >>> [factorial(n) for n in range(6)]
    [1, 1, 2, 6, 24, 120]
    >>> factorial(30)
    265252859812191058636308480000000
    >>> factorial(-1)
    Traceback (most recent call last):
        ...
    ValueError: n must be >= 0

    Factorials of floats are OK, but the float must be an exact integer:
    >>> factorial(30.1)
    Traceback (most recent call last):
        ...
    ValueError: n must be exact integer
    >>> factorial(30.0)
    265252859812191058636308480000000

    It must also not be ridiculously large:
    >>> factorial(1e100)
    Traceback (most recent call last):
        ...
    OverflowError: n too large
    """

    import math
    if not n >= 0:
        raise ValueError("n must be >= 0")
    if math.floor(n) != n:
        raise ValueError("n must be exact integer")
    if n+1 == n:  # catch a value like 1e300
        raise OverflowError("n too large")
    result = 1
    factor = 2
    while factor <= n:
        result *= factor
        factor += 1
    return result


if __name__ == "__main__":
    import doctest
    doctest.testmod()
Si tu ejecutas example.py directamente desde la línea de comandos, doctest hará su magia:

$ python example.py
$
¡No hay salida! Eso es normal, y significa que todos los ejemplos funcionaron. Pasa -v al script, y doctest imprime un registro detallado de lo que está intentando, e imprime un resumen al final:

$ python example.py -v
Trying:
    factorial(5)
Expecting:
    120
ok
Trying:
    [factorial(n) for n in range(6)]
Expecting:
    [1, 1, 2, 6, 24, 120]
ok
Y demás, eventualmente terminando con:

Trying:
    factorial(1e100)
Expecting:
    Traceback (most recent call last):
        ...
    OverflowError: n too large
ok
2 items passed all tests:
   1 tests in __main__
   8 tests in __main__.factorial
9 tests in 2 items.
9 passed and 0 failed.
Test passed.
$
¡Eso es todo lo que necesitas saber para empezar a hacer uso productivo de doctest! Lánzate. Las siguientes secciones proporcionan detalles completos. Note que hay muchos ejemplos de doctests en el conjunto de pruebas estándar de Python y bibliotecas. Especialmente ejemplos útiles se pueden encontrar en el archivo de pruebas estándar Lib/test/test_doctest.py.