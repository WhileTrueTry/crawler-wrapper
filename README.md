# Web Scraper con crawl4ai

[![Python](https://img.shields.io/badge/python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.2-orange.svg)](https://github.com/whiletruetry/crawler-scraper)

Un web scraper potente y flexible construido con crawl4ai que permite extraer contenido de páginas web de forma síncrona y asíncrona, con capacidades de scraping simple y recursivo.
Todo esto es posible gracias a los desarrolladores que crearon y mantienen crawl4ai. 
Si necesitás algo más complejo, probablemente su framework sea útil
No dejes de visitarlos :)
https://docs.crawl4ai.com/


## 🚀 Características

- **Extracción de página individual (landpage)**: Scraping de una sola página web
- **Extracción recursiva (general)**: Scraping de múltiples páginas siguiendo enlaces internos
- **Múltiples formatos de salida**: markdown, markdown estilizado, HTML limpio, HTML completo
- **Guardado automático**: Opción para guardar el contenido extraído en archivos de texto
- **Interfaz de línea de comandos**: CLI completa con múltiples opciones
- **Control de profundidad**: Configurable hasta qué nivel de enlaces seguir
- **Filtrado inteligente**: Solo extrae enlaces del mismo dominio y evita archivos multimedia
- **Markdown estilizado**: Elimina enlaces y URLs del texto markdown
- **Manejo robusto de errores**: Continúa la extracción aunque algunas páginas fallen

## 📦 Instalación

### Requisitos y dependencias

- Python 3.7+

```bash
pip install crawl4ai beautifulsoup4
crawl4ai-setup
```

### Instalación desde el repositorio

```bash
git clone https://github.com/whiletruetry/crawler-wrapper.git
cd crawler-wrapper
pip install -r requirements.txt
```

## 🔧 Uso

### Línea de comandos

#### Extraer una sola página

```bash
# Extracción básica
python scraper.py landpage https://example.com

# Guardar en archivo
python scraper.py landpage https://example.com --save-path mi_sitio

# Usar formato HTML limpio
python scraper.py landpage https://example.com --result-type cleaned_html

# Solo guardar, no mostrar contenido
python scraper.py landpage https://example.com --save-path docs --no-return
```

#### Extraer múltiples páginas (recursivo)

```bash
# Extracción recursiva básica
python scraper.py general https://example.com

# Limitar páginas y profundidad
python scraper.py general https://example.com --max-pages 50 --max-depth 2

# Guardar en markdown limpio
python scraper.py general https://example.com --save-path docs --result-type cleaned_markdown

# Solo guardar archivos
python scraper.py general https://example.com --save-path output --no-return
```

### Uso programático

#### Extracción asíncrona

```python
import asyncio
from scraper import scrap_landpage, scrap_general

async def main():
    # Extraer una página
    content = await scrap_landpage("https://example.com")
    print(content)
    
    # Extraer múltiples páginas
    content_list = await scrap_general(
        "https://example.com", 
        max_pages=20, 
        max_depth=2
    )
    for content in content_list:
        print(f"Página extraída: {len(content)} caracteres")

asyncio.run(main())
```

#### Extracción síncrona

```python
from scraper import scrap_landpage_sync, scrap_general_sync

# Extraer una página
content = scrap_landpage_sync("https://example.com")

# Extraer múltiples páginas y guardar
content_list = scrap_general_sync(
    "https://example.com",
    max_pages=10,
    max_depth=1,
    save_path="mi_scraping",
    result_type="cleaned_markdown"
)
```

## 📋 Opciones de configuración

### Tipos de resultado (`result_type`)

| Tipo | Descripción |
|------|-------------|
| `markdown` | Contenido en formato markdown (predeterminado) |
| `cleaned_markdown` | Markdown sin enlaces `[texto](url)` |
| `html` | HTML completo de la página |
| `cleaned_html`, `fit_html` | versiones más livianas de HTML |

### Parámetros principales

| Parámetro | Descripción | Predeterminado |
|-----------|-------------|----------------|
| `url` | URL de la página o sitio web | Requerido |
| `max_pages` | Número máximo de páginas a extraer | 100 |
| `max_depth` | Profundidad máxima de rastreo | 3 |
| `save_path` | Directorio donde guardar archivos | None |
| `return_content` | Si retornar el contenido extraído | True |
| `result_type` | Formato del contenido extraído | "markdown" |

## 📁 Estructura de archivos guardados

Cuando se especifica `save_path`, los archivos se guardan en:

```
[save_path]/
├── 001_index_markdown.txt
├── 002_about_markdown.txt
└── 003_contact_markdown.txt
```

Cada archivo incluye:
- URL de origen
- Tipo de resultado utilizado
- Contenido extraído

## 🛠️ Ejemplos avanzados


### Scraping de documentación 

```python
import asyncio
from scraper import scrap_general

async def analizar_sitio():
    contenidos = await scrap_general(
        "https://blog.example.com",
        max_pages=50,
        result_type="markdown"
    )
    
    print(f"Páginas extraídas: {len(contenidos)}")
    print(f"Total caracteres: {total_caracteres}")
    print(f"Promedio por página: {promedio_caracteres:.0f}")

asyncio.run(analizar_sitio())
```

### Scraping de documentación con wrapper síncrono

```python
# Extraer documentación de un sitio
content_list = scrap_general_sync(
    "https://docs.example.com",
    max_pages=200,
    max_depth=4,
    save_path="documentacion_completa",
    result_type="cleaned_markdown"
)
```


## ⚠️ Consideraciones importantes

### Limitaciones técnicas
- Solo extrae contenido del mismo dominio
- No ejecuta JavaScript complejo
- Puede tener problemas con sitios que requieren autenticación
- No maneja CAPTCHAs automáticamente


### Errores comunes

**Error: "No module named 'crawl4ai'"**
```bash
pip install crawl4ai
crawl4ai-setup    ## La instalación requiere algunos pasos extra, todo está indicado en un flujo guiado
```

Dependiendo de tu sistema operativo y el ambiente de ejecución, puede haber algunas complicaciones menores en la instalación de crawl4ai. Nada demasiado complicado. 

**Error de timeout o conexión**
- Verifica tu conexión a internet
- Algunos sitios pueden bloquear requests automatizados
- Intenta con un `max_pages` menor

**Contenido vacío o incompleto**
- Prueba diferentes valores de `result_type`
- Algunos sitios cargan contenido dinámicamente con JavaScript



## 🙏 Agradecimientos

- [crawl4ai](https://github.com/unclecode/crawl4ai) - La librería principal de web crawling
- [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) - Para el parsing de HTML
- Comunidad de Python por las increíbles herramientas disponibles



⭐ Si este proyecto te fue útil, ¡considera darle una estrella en GitHub!
