# Python para GIS

Material didático em Jupyter Notebooks para aprender análise geoespacial com Python. O curso é dividido em três notebooks progressivos, partindo dos fundamentos de geometria até um estudo de caso com dados públicos reais do Brasil.

Todos os notebooks podem ser abertos diretamente no Google Colab (link no topo de cada arquivo) ou executados localmente.

## Requisitos e instalação

O projeto usa [uv](https://docs.astral.sh/uv/) para gerenciamento de dependências (ver `pyproject.toml` / `uv.lock`), mas também há um `requirements.txt` para instalação via pip. Principais bibliotecas:

- **Shapely** — geometrias vetoriais (pontos, linhas, polígonos)
- **GeoPandas** — DataFrames com suporte geoespacial
- **Fiona** — leitura/escrita de arquivos geoespaciais (shapefile, GeoJSON, GPKG)
- **geobr** — download de dados públicos do IBGE (municípios, estados, unidades de saúde)
- **Matplotlib** — visualização de mapas
- **pandas / numpy / psutil** — suporte geral e análise de uso de memória

Requer Python >= 3.13.

## Estrutura do repositório

```text
python_gis_0.ipynb   # Aula 0 — Fundamentos: Simple Features e Shapely
python_gis_1.ipynb   # Aula 1 — GeoPandas: de CSV a GeoDataFrame
python_gis_2.ipynb   # Aula 3 — Operações espaciais avançadas, Fiona e estudo de caso
pontos_coleta.csv    # Dados de exemplo (pontos de coleta de água) usados na Aula 1
saida/                # Arquivos exportados pelos notebooks (shapefile, GeoJSON, GeoPackage)
src/gis_python/       # Pacote Python mínimo do projeto (placeholder)
```

## Notebook 0 —  Representando o espaço com geometrias

Introduz os fundamentos teóricos e práticos da representação de dados espaciais:

- **Simple Features / OGC**: o padrão internacional (OGC + ISO) que define como armazenar geometrias geográficas, e os principais padrões relacionados (WMS, WFS, WCS, GML, KML, GeoPackage).
- **Well-Known Text (WKT/WKB)**: a hierarquia de geometrias (`Point`, `LineString`, `Polygon`, `MultiPoint`, `MultiLineString`, `MultiPolygon`, `GeometryCollection`) e as regras de validade topológica.
- **Shapely na prática**: criação de pontos, linhas e polígonos (incluindo polígonos com buracos), inspeção de propriedades (`.area`, `.length`, `.centroid`, `.bounds`, `.geom_type`, `.wkt`, `.is_valid`).
- **Operações geométricas**: `buffer`, `convex_hull`, `simplify` e transformações afins (translação, rotação, escala).
- **Predicados espaciais (topologia)**: os 8 predicados OGC — `equals`, `disjoint`, `intersects`, `touches`, `crosses`, `within`, `contains`, `overlaps` — com visualizações comparativas.
- **Operações de conjunto**: `union`, `intersection`, `difference`, `symmetric_difference` entre geometrias.
- **Formatos WKT e WKB**: representação textual vs. binária de geometrias, usada para depuração e armazenamento em bancos como PostGIS.



## Notebook 1 —  GeoPandas

**Objetivo declarado:** transformar um CSV com colunas de coordenadas em um dado geoespacial, atribuir um sistema de referência (CRS), visualizar no mapa e exportar para formatos GIS.

- **Por que Python para GIS**: comparação com ferramentas desktop (ArcGIS/QGIS) e panorama do ecossistema Python (GeoPandas, Shapely, Fiona, GDAL/Rasterio, Folium/Leafmap).
- **Revisão rápida de Pandas**: DataFrame, Series, `pd.read_csv()`.
- **Criação de um `GeoDataFrame`**: uso de `gpd.points_from_xy()` para converter colunas `x`/`y` de um CSV (`pontos_coleta.csv`, dados fictícios de qualidade de água) em uma coluna de geometria do tipo `Point`.
- **Sistemas de Referência de Coordenadas (CRS)**: diferença entre CRS geográfico (graus, ex. EPSG:4326/WGS84) e projetado (metros, ex. EPSG:31983); a diferença entre WGS84 e SIRGAS2000 (padrão oficial do Brasil); a distinção crucial entre `.set_crs()` (declarar) e `.to_crs()` (converter/reprojetar).
- **Visualização**: plotagem de geometrias com `.plot()` do GeoPandas/Matplotlib.
- **Exportação**: salvar o GeoDataFrame como Shapefile, com tabela comparativa de formatos (Shapefile vs. GeoJSON vs. GeoPackage e quando usar cada um).
- **Integração Shapely + GeoPandas**: construção manual de um GeoDataFrame de zonas urbanas (`Polygon`/`box`), cálculo de centróides, área e interseção entre feições, e um alerta importante sobre calcular área em CRS geográfico (não projetado).
- **Exercícios**: propõe criar um CSV próprio de cidades, converter em GeoDataFrame, atribuir CRS, plotar e exportar como GeoJSON, além de um desafio de reprojeção para UTM.

## Notebook 2 — Operações espaciais

**Objetivo declarado:** dominar operações espaciais mais complexas com GeoPandas, realizar consultas espaciais (spatial queries), aprender a ler/escrever formatos geoespaciais com Fiona, e aplicar tudo num estudo de caso real.

- **Fiona**: interface de baixo nível com a OGR/GDAL para ler e escrever arquivos geoespaciais feição a feição (mais controle sobre schema e memória do que o GeoPandas). Mostra a estrutura de uma fonte Fiona (`driver`, `crs`, `schema`, `bounds`) e como escrever um arquivo do zero usando `shapely.geometry.mapping()` como ponte entre Shapely e Fiona.
- **Spatial Join (`gpd.sjoin`)**: equivalente espacial de um JOIN de SQL — exemplo unindo escolas a bairros pelo predicado `within`, seguido de agregação (`groupby`) para contar escolas e somar alunos por bairro.
- **Overlay (`gpd.overlay`)**: operações de conjunto (`intersection`, `union`, `difference`, `symmetric_difference`, `identity`) aplicadas a camadas inteiras de GeoDataFrames, com exemplo de zonas urbanas e visualização lado a lado.
- **Reprojeção de CRS (`.to_crs()`)**: por que reprojetar para um CRS plano (em metros) antes de calcular buffer, área ou distância; como descobrir o EPSG correto para uma região via epsg.io; lista dos principais EPSG do SIRGAS2000/UTM no Brasil.
- **Dissolve**: agregação espacial que funde geometrias vizinhas que compartilham um valor de atributo (ex. unir quadras em bairros, com soma de população ou contagem de feições).
- **Estudo de caso — Alagoas (dados IBGE via `geobr`)**:
  - Download de malha municipal do estado de Alagoas e de unidades de saúde (`geobr.read_municipality`, `geobr.read_health_facilities`).
  - Verificação de consistência de CRS entre camadas (`assert muni.crs == df_upa.crs`).
  - Checagem de uso de memória (RAM) dos GeoDataFrames com `psutil`.
  - Limpeza de dados: identificação de pontos de unidades de saúde fora dos limites do estado via `dissolve()` + `sjoin`.
  - Agregação final: contagem de unidades de saúde por município.
  - **Exercício resolvido**: mapa coroplético mostrando o número de unidades de saúde por município de Alagoas.


