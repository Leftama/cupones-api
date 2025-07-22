# API de Cupones con Pruebas de Regresión Automatizadas

Este proyecto demuestra cómo construir una API simple en Python usando Flask para aplicar cupones de descuento y cómo detectar regresiones a través de pruebas automatizadas con `pytest` y GitHub Actions.

---

## Objetivos

- Construir una API simple con Flask.
- Implementar pruebas unitarias y de regresión.
- Simular una regresión intencional en el código.
- Automatizar las pruebas con GitHub Actions.
- Mejorar la calidad del software en un flujo ágil.

---

## 📦 Estructura del Proyecto

```text
../cupones-api/
├── app
│   ├── api.py
│   ├── cupones.py
│   ├── __init__.py
│   └── __pycache__
├── README.md
├── requirements.txt
├── tests
│   ├── __pycache__
│   ├── test_api.py
│   └── test_cupones.py
└── venv
    ├── bin
    ├── include
    ├── lib
    ├── lib64 -> lib
    └── pyvenv.cfg
```

---

## Cómo ejecutar localmente

### 1. Clonar el repositorio

```bash
git clone https://github.com/Leftama/cupones-api.git
cd cupones-api
```

### 2. Crear y activar un entorno virtual

```bash
python -m venv venv
source venv/bin/activate
```

### 3. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 4. Ejecutar pruebas

```bash
python -m pytest -v tests/test_cupones.py
```

---

## Simulación de una Regresión

Para probar la efectividad de las pruebas de regresión:

### 1. Editar `app.py` y modificar la lógica del cálculo

```diff
- "BIENVENIDA": 0.15
+ # El cupon "BIENVENIDA" se eliminó sin querer
```

### 2. Ejecutar nuevamente `pytest`. Verás que la prueba falla

```bash
python -m pytest -v tests/test_cupones.py
======================================= test session starts ========================================
platform linux -- Python 3.11.12, pytest-8.4.1, pluggy-1.6.0 -- /home/leftraru/Escritorio/cupones-api/venv/bin/python
cachedir: .pytest_cache
rootdir: /home/leftraru/Escritorio/cupones-api
collected 4 items                                                                                  

tests/test_cupones.py::test_descuento_oferta10 PASSED                                        [ 25%]
tests/test_cupones.py::test_descuento_super20 PASSED                                         [ 50%]
tests/test_cupones.py::test_descuento_bienvenida FAILED                                      [ 75%]
tests/test_cupones.py::test_precio_final_con_impuesto PASSED                                 [100%]

============================================= FAILURES =============================================
____________________________________ test_descuento_bienvenida _____________________________________

    def test_descuento_bienvenida():
>       assert aplicar_cupon(100, "BIENVENIDA") == 85.0
E       AssertionError: assert 100 == 85.0
E        +  where 100 = aplicar_cupon(100, 'BIENVENIDA')

tests/test_cupones.py:7: AssertionError
===================================== short test summary info ======================================
FAILED tests/test_cupones.py::test_descuento_bienvenida - AssertionError: assert 100 == 85.0
=================================== 1 failed, 3 passed in 0.25s ====================================

```

✔️ Esto demuestra que el error fue detectado automáticamente.

---

## Automatización con GitHub Actions

El repositorio incluye un workflow de CI que ejecuta las pruebas automáticamente en cada push o pull request sobre `main`.

### `.github/workflows/test.yml`

```yaml
name: Pruebas de Regresión - Cupones

on:
 push:
   branches: [main]
 pull_request:
   branches: [main]

jobs:
   test:
     runs-on: ubuntu-latest
 
     steps:
       - uses: actions/checkout@v3

       - name: Configurar Python
         uses: actions/setup-python@v4
         with:
           python-version: '3.11.12'
 
       - name: Instalar dependencias
         run: |
           python -m venv venv
           source venv/bin/activate
           pip install -r requirements.txt

       - name: Ejecutar pruebas
         run: |
           source venv/bin/activate
           python -m pytest -v tests/test_cupones.py
```

---

## Preguntas Finales

### 1. ¿Por qué fue difícil detectar la regresión sin pruebas?

Porque el error en la lógica del cálculo del descuento era sutil. Sin pruebas automatizadas, puede pasar desapercibido durante el desarrollo o llegar a producción, lo que puede afectar a los usuarios y a la reputación del software.

### 2. ¿Cómo te ayuda el testing automatizado a mantener calidad?

Las pruebas automatizadas aseguran que las funcionalidades existentes no se rompan cuando se introducen cambios. Permiten detectar errores de forma inmediata, reduciendo el tiempo de depuración y aumentando la confianza en cada entrega.

### 3. ¿Qué otras partes del código deberías cubrir con pruebas?

- Validaciones de datos de entrada (ej. precio negativo, campos faltantes).
- Casos límite (descuento del 100%, cupón vacío, tipo de dato incorrecto).
- Respuestas a métodos HTTP no válidos.
- Comportamiento frente a cupones repetidos o caducados (si se implementara).

### 4. ¿Cómo evitarías errores similares en el futuro?

- Usando TDD (Test-Driven Development).
- Manteniendo una buena cobertura de pruebas automatizadas.
- Revisando código en pull requests con enfoque en la lógica de negocio.
- Automatizando los tests en CI para validación continua.

---

## Licencia

Este proyecto está bajo la licencia MIT.

---

## ✍️ Autor

Desarrollado como ejercicio educativo para la práctica de **Pruebas de Regresión Automatizadas** en proyectos ágiles con CI/CD.

---
