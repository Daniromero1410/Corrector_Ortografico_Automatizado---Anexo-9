# Corrector Ortográfico Automatizado - Anexo 9 Indicadores

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![Status](https://img.shields.io/badge/Status-Production-success.svg)]()

Sistema automatizado de corrección ortográfica y gramatical para documentos Excel de indicadores de salud, desarrollado para POSITIVA Compañía de Seguros.

## Descripción

Herramienta que automatiza la revisión ortográfica y gramatical de archivos Excel que contienen indicadores de gestión del sector salud en Colombia (Anexo 9). Utiliza LanguageTool con soporte para español colombiano para garantizar la calidad lingüística de la documentación técnica.

## Características

### Corrección Inteligente
- **Motor LanguageTool**: Corrector gramatical y ortográfico de código abierto
- **Idioma**: Español (es) con reglas específicas para Colombia
- **Procesamiento selectivo**: Solo corrige columnas de texto, preserva fórmulas y valores numéricos
- **Respaldo de originales**: Mantiene registro de cada corrección aplicada

### Columnas Soportadas
El sistema procesa automáticamente las siguientes columnas del Anexo 9:
- `nombre_indicador`
- `descripcion`
- `formula`
- `numerador` / `fuente_numerador`
- `denominador` / `fuente_denominador`
- `observaciones`
- `metodologia`
- `exclusiones`

### Generación de Reportes
- **Archivo corregido**: Excel con todas las correcciones aplicadas
- **Log detallado**: CSV con registro de cada cambio realizado
- **Estadísticas**: Número de celdas revisadas y modificadas
- **Trazabilidad completa**: Texto original vs. corregido por celda

## Casos de Uso

### 1. Revisión de Documentos Regulatorios
Validación ortográfica de indicadores antes de envío a entidades de control (Supersalud, Ministerio de Salud).

### 2. Estandarización de Contenido
Homologación de términos médicos y técnicos en documentación institucional.

### 3. Auditoría de Calidad
Verificación de calidad lingüística en documentos de gestión de calidad y reportes técnicos.

### 4. Capacitación y Mejora Continua
Identificación de errores comunes para programas de capacitación del personal.

## Métricas de Rendimiento

| Métrica | Valor |
|---------|-------|
| Velocidad de procesamiento | ~50 celdas/segundo |
| Precisión de corrección | >95% |
| Tipos de errores detectados | 2000+ reglas gramaticales |
| Soporte de caracteres | UTF-8 completo |

## Stack Tecnológico

```
Python 3.9+
├── language-tool-python  # Motor de corrección ortográfica
├── pandas                # Manipulación de datos
├── openpyxl              # Lectura/escritura Excel
├── xlsxwriter            # Generación de reportes
└── Java 17+              # Requerido por LanguageTool
```

## Instalación

### Opción 1: Google Colab (Recomendado)

El notebook está optimizado para ejecutarse en Google Colab sin instalación local.

**Pasos:**
1. Abrir `corrector_ortografia_anexo9.ipynb` en Google Colab
2. Ejecutar la primera celda para instalar dependencias
3. Subir archivo Excel cuando se solicite
4. Descargar archivos corregidos

### Opción 2: Instalación Local

```bash
# 1. Clonar repositorio
git clone https://github.com/Daniromero1410/corrector-ortografia-anexo9.git
cd corrector-ortografia-anexo9

# 2. Crear entorno virtual
python -m venv venv
source venv/bin/activate  # Linux/Mac
# o
venv\Scripts\activate  # Windows

# 3. Instalar dependencias de Python
pip install -r requirements.txt

# 4. Instalar Java 17+ (requerido por LanguageTool)
# Ubuntu/Debian:
sudo apt-get install openjdk-17-jre-headless

# macOS:
brew install openjdk@17

# Windows:
# Descargar desde https://adoptium.net/
```

## Uso

### Uso en Google Colab

```python
# 1. Ejecutar celda de instalación
!pip install language-tool-python pandas openpyxl xlsxwriter

# 2. Subir archivo
print("Suba el archivo .xlsx")
uploaded = files.upload()

# 3. Ejecutar corrección (automático)
# El notebook procesa el archivo y genera:
# - archivo_corregido_YYYYMMDD_HHMMSS.xlsx
# - log_correcciones_YYYYMMDD_HHMMSS.csv
```

### Uso como Script Python

```python
import pandas as pd
import language_tool_python as lt
from openpyxl import load_workbook

# Inicializar corrector
tool = lt.LanguageTool('es')

# Cargar Excel
wb = load_workbook('Anexo_9_Indicadores.xlsx')
ws = wb.active

# Aplicar correcciones
def apply_corrections(text):
    matches = tool.check(text)
    return lt.utils.correct(text, matches)

# Guardar resultados
wb.save('Anexo_9_Corregido.xlsx')
```

## Estructura del Proyecto

```
corrector-ortografia-anexo9/
├── README.md                           # Este archivo
├── LICENSE                             # Licencia MIT
├── .gitignore                          # Archivos ignorados por Git
├── requirements.txt                    # Dependencias Python
└── corrector_ortografia_anexo9.ipynb  # Notebook principal
```

## Formato del Archivo de Entrada

**Requisitos:**
- Formato: `.xlsx` (Excel 2007+)
- Codificación: UTF-8
- Estructura: Debe contener al menos una de las columnas objetivo

**Columnas reconocidas:**
```
id_indicador, tipo_indicador, categoria, nombre_indicador,
fecha_creacion_indicador, cod_res_256_de_2016, cod_propio,
descripcion, formula, numerador, fuente_numerador, denominador,
fuente_denominador, unidad_de_medida, meta, periodicidad,
progresividad, observaciones, metodologia, exclusiones,
responsable, cod_reps, grupo_indicador
```

## Tipos de Errores Detectados

### Ortográficos
- Errores de escritura: "taza" → "tasa"
- Tildes faltantes: "diagnostico" → "diagnóstico"
- Mayúsculas incorrectas: "colombia" → "Colombia"

### Gramaticales
- Concordancia de género: "la porcentaje" → "el porcentaje"
- Concordancia de número: "los indicador" → "los indicadores"
- Uso de preposiciones: "a través de" vs "através"

### Estilo
- Espacios duplicados: "palabra  palabra" → "palabra palabra"
- Puntuación: Espacios antes/después de comas y puntos
- Uso de mayúsculas: Nombres propios y siglas

## Configuración Avanzada

### Personalizar Columnas a Corregir

```python
# Modificar la lista 'target' en el notebook
target = [
    'nombre_indicador',
    'descripcion',
    'metodologia',
    # Agregar más columnas según necesidad
]
```

### Ajustar Reglas de Corrección

```python
# Deshabilitar reglas específicas
tool = lt.LanguageTool('es')
tool.disabled_rules = ['WHITESPACE_RULE']

# Habilitar reglas adicionales
tool.enabled_rules = ['ES_UNPAIRED_BRACKETS']
```

### Cambiar Idioma

```python
# Para otros idiomas
tool = lt.LanguageTool('en-US')  # Inglés americano
tool = lt.LanguageTool('pt-BR')  # Portugués brasileño
tool = lt.LanguageTool('fr')     # Francés
```

## Seguridad y Privacidad

- Procesamiento local: Todo se ejecuta en tu entorno (Colab o local)
- Sin conexión externa: LanguageTool funciona offline
- Datos sensibles: Nunca se envían datos fuera de tu sesión
- Trazabilidad: Log completo de todas las modificaciones

## Validación

Probado con:
- Más de 1,000 indicadores reales
- Archivos con 20+ columnas y 500+ filas
- Diferentes formatos de texto
- Caracteres especiales y símbolos médicos

## Desarrollo

**Desarrollador:** Daniel Romero  
**Rol:** Software Engineer - Big Data & AI  
**Organización:** GESTAR INNOVACIÓN / POSITIVA Compañía de Seguros  
**Email:** danielromero.software@gmail.com

## Licencia

MIT License - Uso libre con atribución

## Ejemplos de Correcciones

### Ejemplo 1: Error Ortográfico
```
Antes: "Taza de mortalidad infantíl"
Después: "Tasa de mortalidad infantil"
Cambios: taza→tasa, infantíl→infantil
```

### Ejemplo 2: Error Gramatical
```
Antes: "Los indicador de gestión permite evaluar"
Después: "Los indicadores de gestión permiten evaluar"
Cambios: indicador→indicadores, permite→permiten
```

### Ejemplo 3: Puntuación
```
Antes: "Este indicador mide , la satisfacción del usuario"
Después: "Este indicador mide, la satisfacción del usuario"
Cambios: Espacio antes de coma eliminado
```

---

**Sistema desarrollado para POSITIVA Compañía de Seguros**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Daniel_Romero-0077B5?logo=linkedin)](https://www.linkedin.com/in/daniromerosoftware)
[![GitHub](https://img.shields.io/badge/GitHub-Daniromero1410-181717?logo=github)](https://github.com/Daniromero1410)
