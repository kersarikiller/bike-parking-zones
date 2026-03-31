# bike-parking-zones

Анализ распределения велопарковок по районам Москвы на основе открытых данных OpenStreetMap. Результат — GeoPackage со статистикой по районам и карта зонирования в QGIS.

---

## О проекте

Проект исследует, насколько равномерно велопарковки распределены по городу: в каких районах их много, в каких мало, а в каких нет совсем. Подобная задача возникает при зонировании территории работы городских сервисов — доставки, микромобильности, логистики.

**Что делает ноутбук:**

1. Загружает геоданные велопарковок и границ районов из GeoJSON
2. Приводит оба слоя к единой проекции — EPSG:32637 (UTM зона 37N, оптимальная для Москвы)
3. Фильтрует невалидную геометрию
4. Пространственный джойн (`sjoin`): привязывает каждую велопарковку к району
5. Считает количество велопарковок на район (`groupby` + `size`)
6. Присоединяет статистику к слою районов через `merge`
7. Классифицирует районы на 4 зоны по квантилям: **нет / мало / средне / много**
8. Сохраняет результат в GeoPackage для дальнейшей работы в QGIS

---

## Стек

- **Python**: GeoPandas, Pandas
- **QGIS**: визуализация зон, настройка символики, экспорт карты
- **Данные**: OpenStreetMap (Overpass Turbo / Geofabrik)
- **Форматы**: GeoJSON (входные данные), GeoPackage (результат)

---

## Структура репозитория

```
bike-parking-zones/
├── data/
│   ├── raw/
│   │   ├── bike_parking.geojson     # велопарковки OSM
│   │   └── boundaries.geojson       # границы районов OSM
│   └── processed/
│       └── bound_bike_parking.gpkg  # результат с зонами
├── notebooks/
│   └── analysis.ipynb
├── qgis/
│   └── project.qgz
├── output/
│   └── map.png
└── README.md
```

---

## Как запустить

```bash
# Установить зависимости
pip install geopandas pandas

# Запустить ноутбук
jupyter notebook notebooks/analysis.ipynb
```

Входные файлы (`bike_parking.geojson`, `boundaries.geojson`) должны лежать в `data/raw/`.  
Скачать данные можно с [Overpass Turbo](https://overpass-turbo.eu).

---

## Данные

| Слой | Источник | Теги OSM |
|------|----------|----------|
| Велопарковки | OpenStreetMap | `amenity=bicycle_parking` |
| Границы районов | OpenStreetMap | `admin_level=8`, `boundary=administrative` |

Данные актуальны на момент выгрузки и распространяются под лицензией [ODbL](https://www.openstreetmap.org/copyright).

---

## Автор

[kersarikiller](https://github.com/kersarikiller)
