<template>
<div><button @click="a">点我</button>
    <div id="app">
        <button @click="aa3">点我</button>

        <div id="info">&nbsp;</div> 添加矢量数据的显示信息
        <div id="info">No countries selected</div>

        <div id="map"> </div>
    </div>
</div>
</template>

  
    // <div id="info">&nbsp;</div> 添加矢量数据的显示信息
    //  <div id="info">No countries selected</div>

  <!-- <form class="form-inline"> 绘制贝塞尔曲线
      <label for="shall-smoothen">Smooth drawn geometry?</label>
      <input id="shall-smoothen" type="checkbox" checked><br>
      <label for="iterations">Number of smoothings</label>
      <input style="width: 250px;" type="range" id="iterations" min="2" max="10" step="1" value="5">
    </form> -->

    <!-- <form class="form-inline"> 自由绘制要素
      <label>Geometry type &nbsp;</label>
      <select id="type">
        <option value="LineString">LineString</option>
        <option value="Polygon">Polygon</option>
        <option value="Circle">Circle</option>
        <option value="None">None</option>
      </select>
    </form> -->

  <!-- <form class="form-inline"> 自由绘制要素
      <label>Shape type &nbsp;</label>
      <select id="type">
        <option value="Circle">Circle</option>
        <option value="Square">Square</option>
        <option value="Box">Box</option>
        <option value="Star">Star</option>
        <option value="None">None</option>
      </select>
    </form> --> 

<!-- <select id="mode">编辑折点
      <option value="modify">select a feature to modify</option>
      <option value="draw">draw new features</option>
    </select>   -->
//数据输出
  <a id="export-png" class="btn btn-default"><i class="fa fa-download"></i> Download PNG</a>
  <a id="image-download" download="map.png"></a>

  <!-- <label for="speed">动态展示路线
    speed:&nbsp;
    <input id="speed" type="range" min="10" max="999" step="10" value="60">
  </label>
  <button id="start-animation">Start Animation</button>
    -->

<!--引用UI的js文件-->

<script src="https://unpkg.com/elm-pep"></script>

<script>
//渲染geojson几何图形(成功)
// import 'ol/ol.css';
// import Circle from 'ol/geom/Circle';
// import Feature from 'ol/Feature';
// import GeoJSON from 'ol/format/GeoJSON';
// import Map from 'ol/Map';
// import View from 'ol/View';
// import {Circle as CircleStyle, Fill, Stroke, Style} from 'ol/style';
// import {OSM, Vector as VectorSource} from 'ol/source';
// import {Tile as TileLayer, Vector as VectorLayer} from 'ol/layer';
//渲染点
// import 'ol/ol.css';
// import Feature from 'ol/Feature';
// import Map from 'ol/Map';
// import Point from 'ol/geom/Point';
// import Stamen from 'ol/source/Stamen';
// import TileLayer from 'ol/layer/Tile';
// import View from 'ol/View';
// import WebGLPointsLayer from 'ol/layer/WebGLPoints';
// import {Vector} from 'ol/source';
// import {fromLonLat} from 'ol/proj';

//拖拽矢量数据+选择要素并高亮(成功)
import 'ol/ol.css';
import Map from 'ol/Map';
import View from 'ol/View';
import {
    DragAndDrop,
    defaults as defaultInteractions,
} from 'ol/interaction';
import {
    GPX,
    GeoJSON,
    IGC,
    KML,
    TopoJSON
} from 'ol/format';
import {
    Tile as TileLayer,
    VectorImage as VectorImageLayer,
} from 'ol/layer';
import {
    Vector as VectorSource,
    XYZ
} from 'ol/source';

import {
    DragBox,
    Select
} from 'ol/interaction';
import {
    OSM
} from 'ol/source';
import {
    Vector as VectorLayer
} from 'ol/layer';
import {
    platformModifierKeyOnly
} from 'ol/events/condition';

//import axios from 'axios'
//绘制贝塞尔曲线(成功)
// import 'ol/ol.css';
// import Draw from 'ol/interaction/Draw';
// import Map from 'ol/Map';
// import View from 'ol/View';
// import {OSM, Vector as VectorSource} from 'ol/source';
// import {Tile as TileLayer, Vector as VectorLayer} from 'ol/layer';

// import smooth from 'chaikin-smooth';

//自由绘制要素(成功)
// import 'ol/ol.css';
// import Draw from 'ol/interaction/Draw';
// import Map from 'ol/Map';
// import View from 'ol/View';
// import {OSM, Vector as VectorSource} from 'ol/source';
// import {Tile as TileLayer, Vector as VectorLayer} from 'ol/layer';

//自由绘制图形
// import 'ol/ol.css';
// import Draw, {
//   createBox,
//   createRegularPolygon,
// } from 'ol/interaction/Draw';
// import Map from 'ol/Map';
// import Polygon from 'ol/geom/Polygon';
// import View from 'ol/View';
// import {OSM, Vector as VectorSource} from 'ol/source';
// import {Tile as TileLayer, Vector as VectorLayer} from 'ol/layer';

//编辑要素的折点

// import 'ol/ol.css';
// import GeoJSON from 'ol/format/GeoJSON';
// import {Draw, Modify, Select, Snap} from 'ol/interaction';
// import {Map, View} from 'ol/index';
// import {OSM, Vector as VectorSource} from 'ol/source';
// import {Tile as TileLayer, Vector as VectorLayer} from 'ol/layer';
// import {useGeographic} from 'ol/proj';

//下载当前窗口的地图图片
// import 'ol/ol.css';
// import GeoJSON from 'ol/format/GeoJSON';
// import Map from 'ol/Map';
// import View from 'ol/View';
// import {OSM, Vector as VectorSource} from 'ol/source';
// import {Tile as TileLayer, Vector as VectorLayer} from 'ol/layer';

//动态展示路线
// import 'ol/ol.css';
// import Feature from 'ol/Feature';
// import Map from 'ol/Map';
// import Point from 'ol/geom/Point';
// import Polyline from 'ol/format/Polyline';
// import VectorSource from 'ol/source/Vector';
// import View from 'ol/View';
// import XYZ from 'ol/source/XYZ';
// import {
//   Circle as CircleStyle,
//   Fill,
//   Icon,
//   Stroke,
//   Style,
// } from 'ol/style';
// import {  as TileLayer, Vector as VectorLayer} from 'ol/layer';
// import {getVectorContext} from 'ol/render';

export default {
    data() {
        return {
            //设置静态资源公共的基本路径
            publicPath: process.env.BASE_URL
            //加载点数据不成功添加的部分
            // style:null,
            // map:null

        }
    },
    methods: {
        a() {
            var d = document.getElementById('map');
            d.innerHTML = '';
        },
        aa1() {
            var image = new CircleStyle({
                radius: 5,
                fill: null,
                stroke: new Stroke({
                    color: 'red',
                    width: 1
                }),
            });

            var styles = {
                'Point': new Style({
                    image: image,
                }),
                'LineString': new Style({
                    stroke: new Stroke({
                        color: 'green',
                        width: 1,
                    }),
                }),
                'MultiLineString': new Style({
                    stroke: new Stroke({
                        color: 'green',
                        width: 1,
                    }),
                }),
                'MultiPoint': new Style({
                    image: image,
                }),
                'MultiPolygon': new Style({
                    stroke: new Stroke({
                        color: 'yellow',
                        width: 1,
                    }),
                    fill: new Fill({
                        color: 'rgba(255, 255, 0, 0.1)',
                    }),
                }),
                'Polygon': new Style({
                    stroke: new Stroke({
                        color: 'blue',
                        lineDash: [4],
                        width: 3,
                    }),
                    fill: new Fill({
                        color: 'rgba(0, 0, 255, 0.1)',
                    }),
                }),
                'GeometryCollection': new Style({
                    stroke: new Stroke({
                        color: 'magenta',
                        width: 2,
                    }),
                    fill: new Fill({
                        color: 'magenta',
                    }),
                    image: new CircleStyle({
                        radius: 10,
                        fill: null,
                        stroke: new Stroke({
                            color: 'magenta',
                        }),
                    }),
                }),
                'Circle': new Style({
                    stroke: new Stroke({
                        color: 'red',
                        width: 2,
                    }),
                    fill: new Fill({
                        color: 'rgba(255,0,0,0.2)',
                    }),
                }),
            };

            var styleFunction = function (feature) {
                return styles[feature.getGeometry().getType()];
            };

            var geojsonObject = {
                'type': 'FeatureCollection',
                'crs': {
                    'type': 'name',
                    'properties': {
                        'name': 'EPSG:3857',
                    },
                },
                'features': [{
                        'type': 'Feature',
                        'geometry': {
                            'type': 'Point',
                            'coordinates': [0, 0],
                        },
                    },
                    {
                        'type': 'Feature',
                        'geometry': {
                            'type': 'LineString',
                            'coordinates': [
                                [4e6, -2e6],
                                [8e6, 2e6]
                            ],
                        },
                    },
                    {
                        'type': 'Feature',
                        'geometry': {
                            'type': 'LineString',
                            'coordinates': [
                                [4e6, 2e6],
                                [8e6, -2e6]
                            ],
                        },
                    },
                    {
                        'type': 'Feature',
                        'geometry': {
                            'type': 'Polygon',
                            'coordinates': [
                                [
                                    [-5e6, -1e6],
                                    [-4e6, 1e6],
                                    [-3e6, -1e6]
                                ]
                            ],
                        },
                    },
                    {
                        'type': 'Feature',
                        'geometry': {
                            'type': 'MultiLineString',
                            'coordinates': [
                                [
                                    [-1e6, -7.5e5],
                                    [-1e6, 7.5e5]
                                ],
                                [
                                    [1e6, -7.5e5],
                                    [1e6, 7.5e5]
                                ],
                                [
                                    [-7.5e5, -1e6],
                                    [7.5e5, -1e6]
                                ],
                                [
                                    [-7.5e5, 1e6],
                                    [7.5e5, 1e6]
                                ]
                            ],
                        },
                    },
                    {
                        'type': 'Feature',
                        'geometry': {
                            'type': 'MultiPolygon',
                            'coordinates': [
                                [
                                    [
                                        [-5e6, 6e6],
                                        [-5e6, 8e6],
                                        [-3e6, 8e6],
                                        [-3e6, 6e6]
                                    ]
                                ],
                                [
                                    [
                                        [-2e6, 6e6],
                                        [-2e6, 8e6],
                                        [0, 8e6],
                                        [0, 6e6]
                                    ]
                                ],
                                [
                                    [
                                        [1e6, 6e6],
                                        [1e6, 8e6],
                                        [3e6, 8e6],
                                        [3e6, 6e6]
                                    ]
                                ]
                            ],
                        },
                    },
                    {
                        'type': 'Feature',
                        'geometry': {
                            'type': 'GeometryCollection',
                            'geometries': [{
                                    'type': 'LineString',
                                    'coordinates': [
                                        [-5e6, -5e6],
                                        [0, -5e6]
                                    ],
                                },
                                {
                                    'type': 'Point',
                                    'coordinates': [4e6, -5e6],
                                },
                                {
                                    'type': 'Polygon',
                                    'coordinates': [
                                        [
                                            [1e6, -6e6],
                                            [2e6, -4e6],
                                            [3e6, -6e6]
                                        ]
                                    ],
                                }
                            ],
                        },
                    }
                ],
            };

            var vectorSource = new VectorSource({
                features: new GeoJSON().readFeatures(geojsonObject),
            });

            vectorSource.addFeature(new Feature(new Circle([5e6, 7e6], 1e6)));

            var vectorLayer = new VectorLayer({
                source: vectorSource,
                style: styleFunction,
            });

            var map = new Map({
                layers: [
                    new TileLayer({
                        source: new OSM(),
                    }),
                    vectorLayer
                ],
                target: 'map',
                view: new View({
                    center: [0, 0],
                    zoom: 2,
                }),
            });
        },
        aa2() {
            var vectorSource = new Vector({
                attributions: 'NASA',
            });

            var oldColor = 'rgba(242,56,22,0.61)';
            var newColor = '#ffe52c';
            var period = 12; // animation period in seconds
            var animRatio = [
                '^',
                [
                    '/',
                    [
                        '%',
                        [
                            '+',
                            ['time'],
                            ['interpolate', ['linear'],
                                ['get', 'year'], 1850, 0, 2015, period
                            ]
                        ],
                        period
                    ],
                    period
                ],
                0.5
            ];

            var style = {
                variables: {
                    minYear: 1850,
                    maxYear: 2015,
                },
                filter: ['between', ['get', 'year'],
                    ['var', 'minYear'],
                    ['var', 'maxYear']
                ],
                symbol: {
                    symbolType: 'circle',
                    size: [
                        '*',
                        ['interpolate', ['linear'],
                            ['get', 'mass'], 0, 8, 200000, 26
                        ],
                        ['-', 1.75, ['*', animRatio, 0.75]]
                    ],
                    color: ['interpolate', ['linear'], animRatio, 0, newColor, 1, oldColor],
                    opacity: ['-', 1.0, ['*', animRatio, 0.75]],
                },
            };

            this.style = style;
            // handle input values & events
            var minYearInput = document.getElementById('min-year');
            var maxYearInput = document.getElementById('max-year');

            minYearInput.addEventListener('input', this.updateMinYear);
            minYearInput.addEventListener('change', this.updateMinYear);
            maxYearInput.addEventListener('input', this.updateMaxYear);
            maxYearInput.addEventListener('change', this.updateMaxYear);
            this.updateStatusText();

            // load data
            var client = new XMLHttpRequest();
            client.open('GET', './assets/meteorite_landings.csv');
            client.onload = function () {
                var csv = client.responseText;
                var features = [];

                var prevIndex = csv.indexOf('\n') + 1; // scan past the header line

                var curIndex;
                while ((curIndex = csv.indexOf('\n', prevIndex)) != -1) {
                    var line = csv.substr(prevIndex, curIndex - prevIndex).split(',');
                    prevIndex = curIndex + 1;

                    var coords = fromLonLat([parseFloat(line[4]), parseFloat(line[3])]);
                    if (isNaN(coords[0]) || isNaN(coords[1])) {
                        // guard against bad data
                        continue;
                    }

                    features.push(
                        new Feature({
                            mass: parseFloat(line[1]) || 0,
                            year: parseInt(line[2]) || 0,
                            geometry: new Point(coords),
                        })
                    );
                }

                vectorSource.addFeatures(features);
            };
            client.send();

            var map = new Map({
                layers: [
                    new TileLayer({
                        source: new Stamen({
                            layer: 'toner',
                        }),
                    }),
                    new WebGLPointsLayer({
                        style: style,
                        source: vectorSource,
                        disableHitDetection: true,
                    })
                ],
                target: 'map',
                view: new View({
                    center: [0, 0],
                    zoom: 2,
                }),
            });

            this.map = map;
            this.animate();

        },

        //拖拽矢量数据并显示+选择要素并高亮显示
        aa3() {
            var dragAndDropInteraction = new DragAndDrop({
                formatConstructors: [GPX, GeoJSON, IGC, KML, TopoJSON],
            });

            //var key = 'Get your own API key at https://www.maptiler.com/cloud/';
            var attributions =
                '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> ' +
                '<a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OpenStreetMap contributors</a>';

            var map = new Map({
                interactions: defaultInteractions().extend([dragAndDropInteraction]),
                layers: [
                    new TileLayer({
                        source: new XYZ({
                            attributions: attributions,
                            url: 'http://t3.tianditu.com/DataServer?T=img_w&x={x}&y={y}&l={z}&tk=d0cf74b31931aab68af181d23fa23d8d',
                            maxZoom: 20,
                        }),
                    })
                ],
                target: 'map',
                view: new View({
                    center: [0, 0],
                    zoom: 2,
                }),
            });

            dragAndDropInteraction.on('addfeatures', function (event) {
                var vectorSource = new VectorSource({
                    features: event.features,
                });
                map.addLayer(
                    new VectorImageLayer({
                        source: vectorSource,
                    })
                );
                map.getView().fit(vectorSource.getExtent());
            });

            var displayFeatureInfo = function (pixel) {
                var features = [];
                map.forEachFeatureAtPixel(pixel, function (feature) {
                    features.push(feature);
                });
                if (features.length > 0) {
                    var info = [];
                    var i, ii;
                    for (i = 0, ii = features.length; i < ii; ++i) {
                        info.push(features[i].get('name'));
                    }
                    document.getElementById('info').innerHTML = info.join(', ') || '&nbsp';
                } else {
                    document.getElementById('info').innerHTML = '&nbsp;';
                }
            };

            map.on('pointermove', function (evt) {
                if (evt.dragging) {
                    return;
                }
                var pixel = map.getEventPixel(evt.originalEvent);
                displayFeatureInfo(pixel);
            });

            map.on('click', function (evt) {
                displayFeatureInfo(evt.pixel);
            });

            // a normal select interaction to handle click
            var select = new Select();
            map.addInteraction(select);

            var selectedFeatures = select.getFeatures();

            // a DragBox interaction used to select features by drawing boxes
            var dragBox = new DragBox({
                condition: platformModifierKeyOnly,
            });

            map.addInteraction(dragBox);

            dragBox.on('boxend', function () {
                // features that intersect the box geometry are added to the
                // collection of selected features

                // if the view is not obliquely rotated the box geometry and
                // its extent are equalivalent so intersecting features can
                // be added directly to the collection
                var rotation = map.getView().getRotation();
                var oblique = rotation % (Math.PI / 2) !== 0;
                var candidateFeatures = oblique ? [] : selectedFeatures;
                var extent = dragBox.getGeometry().getExtent();
                vectorSource.forEachFeatureIntersectingExtent(extent, function (feature) {
                    candidateFeatures.push(feature);
                });

                // when the view is obliquely rotated the box extent will
                // exceed its geometry so both the box and the candidate
                // feature geometries are rotated around a common anchor
                // to confirm that, with the box geometry aligned with its
                // extent, the geometries intersect
                if (oblique) {
                    var anchor = [0, 0];
                    var geometry = dragBox.getGeometry().clone();
                    geometry.rotate(-rotation, anchor);
                    var extent$1 = geometry.getExtent();
                    candidateFeatures.forEach(function (feature) {
                        var geometry = feature.getGeometry().clone();
                        geometry.rotate(-rotation, anchor);
                        if (geometry.intersectsExtent(extent$1)) {
                            selectedFeatures.push(feature);
                        }
                    });
                }
            });

            // clear selection when drawing a new box and when clicking on the map
            dragBox.on('boxstart', function () {
                selectedFeatures.clear();
            });

            var infoBox = document.getElementById('info');

            selectedFeatures.on(['add', 'remove'], function () {
                var names = selectedFeatures.getArray().map(function (feature) {
                    return feature.get('name');
                });
                if (names.length > 0) {
                    infoBox.innerHTML = names.join(', ');
                } else {
                    infoBox.innerHTML = 'No countries selected';
                }
            });

        },

        //绘制贝塞尔曲线
        aa4() {
            var vectorSource = new VectorSource({});

            var map = new Map({
                layers: [
                    new TileLayer({
                        source: new OSM(),
                        opacity: 0.5,
                    }),
                    new VectorLayer({
                        source: vectorSource,
                    })
                ],
                target: 'map',
                view: new View({
                    center: [1078373.595, 6871994.591],
                    zoom: 5,
                }),
            });

            var shallSmoothen = document.getElementById('shall-smoothen');
            var numIterations = document.getElementById('iterations');

            var makeSmooth = (path, numIterations) => {
                numIterations = Math.min(Math.max(numIterations, 1), 10);
                while (numIterations > 0) {
                    path = smooth(path);
                    numIterations--;
                }
                return path;
            }

            var draw = new Draw({
                source: vectorSource,
                type: 'LineString',
            });
            map.addInteraction(draw);
            draw.on('drawend', function (event) {
                if (!shallSmoothen.checked) {
                    return;
                }
                var feat = event.feature;
                var geometry = feat.getGeometry();
                var coords = geometry.getCoordinates();
                var smoothened = makeSmooth(coords, parseInt(numIterations.value, 10) || 5);
                geometry.setCoordinates(smoothened);
            });

        },

        //自由绘制要素
        aa5() {
            var raster = new TileLayer({
                source: new OSM(),
            });

            var source = new VectorSource({
                wrapX: false
            });

            var vector = new VectorLayer({
                source: source,
            });

            var map = new Map({
                layers: [raster, vector],
                target: 'map',
                view: new View({
                    center: [-11000000, 4600000],
                    zoom: 4,
                }),
            });

            var typeSelect = document.getElementById('type');

            var draw; // global so we can remove it later
            function addInteraction() {
                var value = typeSelect.value;
                if (value !== 'None') {
                    draw = new Draw({
                        source: source,
                        type: typeSelect.value,
                        freehand: true,
                    });
                    map.addInteraction(draw);
                }
            }

            /**
             * Handle change event.
             */
            typeSelect.onchange = function () {
                map.removeInteraction(draw);
                addInteraction();
            };

            addInteraction();

        },

        //自由绘制要素图形
        aa6() {

            var raster = new TileLayer({
                source: new OSM(),
            });

            var source = new VectorSource({
                wrapX: false
            });

            var vector = new VectorLayer({
                source: source,
            });

            var map = new Map({
                layers: [raster, vector],
                target: 'map',
                view: new View({
                    center: [-11000000, 4600000],
                    zoom: 4,
                }),
            });

            var typeSelect = document.getElementById('type');

            var draw; // global so we can remove it later
            function addInteraction() {
                var value = typeSelect.value;
                if (value !== 'None') {
                    var geometryFunction;
                    if (value === 'Square') {
                        value = 'Circle';
                        geometryFunction = createRegularPolygon(4);
                    } else if (value === 'Box') {
                        value = 'Circle';
                        geometryFunction = createBox();
                    } else if (value === 'Star') {
                        value = 'Circle';
                        geometryFunction = function (coordinates, geometry) {
                            var center = coordinates[0];
                            var last = coordinates[1];
                            var dx = center[0] - last[0];
                            var dy = center[1] - last[1];
                            var radius = Math.sqrt(dx * dx + dy * dy);
                            var rotation = Math.atan2(dy, dx);
                            var newCoordinates = [];
                            var numPoints = 12;
                            for (var i = 0; i < numPoints; ++i) {
                                var angle = rotation + (i * 2 * Math.PI) / numPoints;
                                var fraction = i % 2 === 0 ? 1 : 0.5;
                                var offsetX = radius * fraction * Math.cos(angle);
                                var offsetY = radius * fraction * Math.sin(angle);
                                newCoordinates.push([center[0] + offsetX, center[1] + offsetY]);
                            }
                            newCoordinates.push(newCoordinates[0].slice());
                            if (!geometry) {
                                geometry = new Polygon([newCoordinates]);
                            } else {
                                geometry.setCoordinates([newCoordinates]);
                            }
                            return geometry;
                        };
                    }
                    draw = new Draw({
                        source: source,
                        type: value,
                        geometryFunction: geometryFunction,
                    });
                    map.addInteraction(draw);
                }
            }

            /**
             * Handle change event.
             */
            typeSelect.onchange = function () {
                map.removeInteraction(draw);
                addInteraction();
            };

            addInteraction();

        },

        //先自己绘制多边形再修改折点 ，必须自己绘制
        aa7() {

            useGeographic();

            var source = new VectorSource({
                url: '../public/countries.geojson',
                format: new GeoJSON(),
            });

            var map = new Map({
                target: 'map',
                layers: [
                    new TileLayer({
                        source: new OSM(),
                    }),
                    new VectorLayer({
                        source: source,
                    })
                ],
                view: new View({
                    center: [0, 0],
                    zoom: 2,
                }),
            });

            var select = new Select();

            var modify = new Modify({
                features: select.getFeatures(),
            });

            var draw = new Draw({
                type: 'Polygon',
                source: source,
            });

            var snap = new Snap({
                source: source,
            });

            function removeInteractions() {
                map.removeInteraction(modify);
                map.removeInteraction(select);
                map.removeInteraction(draw);
                map.removeInteraction(select);
            }

            var mode = document.getElementById('mode');

            function onChange() {
                removeInteractions();
                switch (mode.value) {
                    case 'draw': {
                        map.addInteraction(draw);
                        map.addInteraction(snap);
                        break;
                    }
                    case 'modify': {
                        map.addInteraction(select);
                        map.addInteraction(modify);
                        map.addInteraction(snap);
                        break;
                    }
                    default: {
                        // pass
                    }
                }
            }
            mode.addEventListener('change', onChange);
            onChange();
        },

        //下载当前窗口的地图图片
        aa8() {
            var map = new Map({
                layers: [
                    new TileLayer({
                        source: new OSM(),
                    }),
                    new VectorLayer({
                        source: new VectorSource({
                            url: 'http://localhost:8080/public/countries.geojson',
                            format: new GeoJSON(),
                        }),
                        opacity: 0.5,
                    })
                ],
                target: 'map',
                view: new View({
                    center: [0, 0],
                    zoom: 2,
                }),
            });

            document.getElementById('export-png').addEventListener('click', function () {
                map.once('rendercomplete', function () {
                    var mapCanvas = document.createElement('canvas');
                    var size = map.getSize();
                    mapCanvas.width = size[0];
                    mapCanvas.height = size[1];
                    var mapContext = mapCanvas.getContext('2d');
                    Array.prototype.forEach.call(
                        document.querySelectorAll('.ol-layer canvas'),
                        function (canvas) {
                            if (canvas.width > 0) {
                                var opacity = canvas.parentNode.style.opacity;
                                mapContext.globalAlpha = opacity === '' ? 1 : Number(opacity);
                                var transform = canvas.style.transform;
                                // Get the transform parameters from the style's transform matrix
                                var matrix = transform
                                    .match(/^matrix\(([^\(]*)\)$/)[1]
                                    .split(',')
                                    .map(Number);
                                // Apply the transform to the export map context
                                CanvasRenderingContext2D.prototype.setTransform.apply(
                                    mapContext,
                                    matrix
                                );
                                mapContext.drawImage(canvas, 0, 0);
                            }
                        }
                    );
                    if (navigator.msSaveBlob) {
                        // link download attribuute does not work on MS browsers
                        navigator.msSaveBlob(mapCanvas.msToBlob(), 'map.png');
                    } else {
                        var link = document.getElementById('image-download');
                        link.href = mapCanvas.toDataURL();
                        link.click();
                    }
                });
                map.renderSync();
            });
        },
        //动态展示路线
        aa9() {
            // This long string is placed here due to jsFiddle limitations.
            // It is usually loaded with AJAX.
            var polyline = [
                'hldhx@lnau`BCG_EaC??cFjAwDjF??uBlKMd@}@z@??aC^yk@z_@se@b[wFdE??wFfE}N',
                'fIoGxB_I\\gG}@eHoCyTmPqGaBaHOoD\\??yVrGotA|N??o[N_STiwAtEmHGeHcAkiA}^',
                'aMyBiHOkFNoI`CcVvM??gG^gF_@iJwC??eCcA]OoL}DwFyCaCgCcCwDcGwHsSoX??wI_E',
                'kUFmq@hBiOqBgTwS??iYse@gYq\\cp@ce@{vA}s@csJqaE}{@iRaqE{lBeRoIwd@_T{]_',
                'Ngn@{PmhEwaA{SeF_u@kQuyAw]wQeEgtAsZ}LiCarAkVwI}D??_}RcjEinPspDwSqCgs@',
                'sPua@_OkXaMeT_Nwk@ob@gV}TiYs[uTwXoNmT{Uyb@wNg]{Nqa@oDgNeJu_@_G}YsFw]k',
                'DuZyDmm@i_@uyIJe~@jCg|@nGiv@zUi_BfNqaAvIow@dEed@dCcf@r@qz@Egs@{Acu@mC',
                'um@yIey@gGig@cK_m@aSku@qRil@we@{mAeTej@}Tkz@cLgr@aHko@qOmcEaJw~C{w@ka',
                'i@qBchBq@kmBS{kDnBscBnFu_Dbc@_~QHeU`IuyDrC_}@bByp@fCyoA?qMbD}{AIkeAgB',
                'k_A_A{UsDke@gFej@qH{o@qGgb@qH{`@mMgm@uQus@kL{_@yOmd@ymBgwE}x@ouBwtA__',
                'DuhEgaKuWct@gp@cnBii@mlBa_@}|Asj@qrCg^eaC}L{dAaJ_aAiOyjByH{nAuYu`GsAw',
                'Xyn@ywMyOyqD{_@cfIcDe}@y@aeBJmwA`CkiAbFkhBlTgdDdPyiB`W}xDnSa}DbJyhCrX',
                'itAhT}x@bE}Z_@qW_Kwv@qKaaAiBgXvIm}A~JovAxCqW~WanB`XewBbK{_A`K}fBvAmi@',
                'xBycBeCauBoF}}@qJioAww@gjHaPopA_NurAyJku@uGmi@cDs[eRaiBkQstAsQkcByNma',
                'CsK_uBcJgbEw@gkB_@ypEqDoqSm@eZcDwjBoGw`BoMegBaU_`Ce_@_uBqb@ytBwkFqiT_',
                'fAqfEwe@mfCka@_eC_UmlB}MmaBeWkkDeHwqAoX}~DcBsZmLcxBqOwqE_DkyAuJmrJ\\o',
                '~CfIewG|YibQxBssB?es@qGciA}RorAoVajA_nAodD{[y`AgPqp@mKwr@ms@umEaW{dAm',
                'b@umAw|@ojBwzDaaJsmBwbEgdCsrFqhAihDquAi`Fux@}_Dui@_eB_u@guCuyAuiHukA_',
                'lKszAu|OmaA{wKm}@clHs_A_rEahCssKo\\sgBsSglAqk@yvDcS_wAyTwpBmPc|BwZknF',
                'oFscB_GsaDiZmyMyLgtHgQonHqT{hKaPg}Dqq@m~Hym@c`EuiBudIabB{hF{pWifx@snA',
                'w`GkFyVqf@y~BkoAi}Lel@wtc@}`@oaXi_C}pZsi@eqGsSuqJ|Lqeb@e]kgPcaAu}SkDw',
                'zGhn@gjYh\\qlNZovJieBqja@ed@siO{[ol\\kCmjMe\\isHorCmec@uLebB}EqiBaCg}',
                '@m@qwHrT_vFps@kkI`uAszIrpHuzYxx@e{Crw@kpDhN{wBtQarDy@knFgP_yCu\\wyCwy',
                'A{kHo~@omEoYmoDaEcPiuAosDagD}rO{{AsyEihCayFilLaiUqm@_bAumFo}DgqA_uByi',
                '@swC~AkzDlhA}xEvcBa}Cxk@ql@`rAo|@~bBq{@``Bye@djDww@z_C_cAtn@ye@nfC_eC',
                '|gGahH~s@w}@``Fi~FpnAooC|u@wlEaEedRlYkrPvKerBfYs}Arg@m}AtrCkzElw@gjBb',
                'h@woBhR{gCwGkgCc[wtCuOapAcFoh@uBy[yBgr@c@iq@o@wvEv@sp@`FajBfCaq@fIipA',
                'dy@ewJlUc`ExGuaBdEmbBpBssArAuqBBg}@s@g{AkB{bBif@_bYmC}r@kDgm@sPq_BuJ_',
                's@{X_{AsK_d@eM{d@wVgx@oWcu@??aDmOkNia@wFoSmDyMyCkPiBePwAob@XcQ|@oNdCo',
                'SfFwXhEmOnLi\\lbAulB`X_d@|k@au@bc@oc@bqC}{BhwDgcD`l@ed@??bL{G|a@eTje@',
                'oS~]cLr~Bgh@|b@}Jv}EieAlv@sPluD{z@nzA_]`|KchCtd@sPvb@wSb{@ko@f`RooQ~e',
                '[upZbuIolI|gFafFzu@iq@nMmJ|OeJn^{Qjh@yQhc@uJ~j@iGdd@kAp~BkBxO{@|QsAfY',
                'gEtYiGd]}Jpd@wRhVoNzNeK`j@ce@vgK}cJnSoSzQkVvUm^rSgc@`Uql@xIq\\vIgg@~k',
                'Dyq[nIir@jNoq@xNwc@fYik@tk@su@neB}uBhqEesFjoGeyHtCoD|D}Ed|@ctAbIuOzqB',
                '_}D~NgY`\\um@v[gm@v{Cw`G`w@o{AdjAwzBh{C}`Gpp@ypAxn@}mAfz@{bBbNia@??jI',
                'ab@`CuOlC}YnAcV`@_^m@aeB}@yk@YuTuBg^uCkZiGk\\yGeY}Lu_@oOsZiTe[uWi[sl@',
                'mo@soAauAsrBgzBqgAglAyd@ig@asAcyAklA}qAwHkGi{@s~@goAmsAyDeEirB_{B}IsJ',
                'uEeFymAssAkdAmhAyTcVkFeEoKiH}l@kp@wg@sj@ku@ey@uh@kj@}EsFmG}Jk^_r@_f@m',
                '~@ym@yjA??a@cFd@kBrCgDbAUnAcBhAyAdk@et@??kF}D??OL'
            ].join('');

            var route = /** @type {import("../src/ol/geom/LineString.js").default} */ (new Polyline({
                factor: 1e6,
            }).readGeometry(polyline, {
                dataProjection: 'EPSG:4326',
                featureProjection: 'EPSG:3857',
            }));

            var routeCoords = route.getCoordinates();
            var routeLength = routeCoords.length;

            var routeFeature = new Feature({
                type: 'route',
                geometry: route,
            });
            var geoMarker = /** @type Feature<import("../src/ol/geom/Point").default> */ (new Feature({
                type: 'geoMarker',
                geometry: new Point(routeCoords[0]),
            }));
            var startMarker = new Feature({
                type: 'icon',
                geometry: new Point(routeCoords[0]),
            });
            var endMarker = new Feature({
                type: 'icon',
                geometry: new Point(routeCoords[routeLength - 1]),
            });

            var styles = {
                'route': new Style({
                    stroke: new Stroke({
                        width: 6,
                        color: [237, 212, 0, 0.8],
                    }),
                }),
                'icon': new Style({
                    image: new Icon({
                        anchor: [0.5, 1],
                        src: 'data/icon.png',
                    }),
                }),
                'geoMarker': new Style({
                    image: new CircleStyle({
                        radius: 7,
                        fill: new Fill({
                            color: 'black'
                        }),
                        stroke: new Stroke({
                            color: 'white',
                            width: 2,
                        }),
                    }),
                }),
            };

            var animating = false;
            var speed, now;
            var speedInput = document.getElementById('speed');
            var startButton = document.getElementById('start-animation');

            var vectorLayer = new VectorLayer({
                source: new VectorSource({
                    features: [routeFeature, geoMarker, startMarker, endMarker],
                }),
                style: function (feature) {
                    // hide geoMarker if animation is active
                    if (animating && feature.get('type') === 'geoMarker') {
                        return null;
                    }
                    return styles[feature.get('type')];
                },
            });

            var key = 'Get your own API key at https://www.maptiler.com/cloud/';
            var attributions =
                '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> ' +
                '<a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OpenStreetMap contributors</a>';

            var center = [-5639523.95, -3501274.52];
            var map = new Map({
                target: document.getElementById('map'),
                view: new View({
                    center: center,
                    zoom: 10,
                    minZoom: 2,
                    maxZoom: 19,
                }),
                layers: [
                    new TileLayer({
                        source: new XYZ({
                            attributions: attributions,
                            url: 'http://t3.tianditu.com/DataServer?T=img_w&x={x}&y={y}&l={z}&tk=d0cf74b31931aab68af181d23fa23d8d',
                            tileSize: 512,
                        }),
                    }),
                    vectorLayer
                ],
            });

            var moveFeature = function (event) {
                var vectorContext = getVectorContext(event);
                var frameState = event.frameState;

                if (animating) {
                    var elapsedTime = frameState.time - now;
                    // here the trick to increase speed is to jump some indexes
                    // on lineString coordinates
                    var index = Math.round((speed * elapsedTime) / 1000);

                    if (index >= routeLength) {
                        stopAnimation(true);
                        return;
                    }

                    var currentPoint = new Point(routeCoords[index]);
                    var feature = new Feature(currentPoint);
                    vectorContext.drawFeature(feature, styles.geoMarker);
                }
                // tell OpenLayers to continue the postrender animation
                map.render();
            };

            function startAnimation() {
                if (animating) {
                    stopAnimation(false);
                } else {
                    animating = true;
                    now = new Date().getTime();
                    speed = speedInput.value;
                    startButton.textContent = 'Cancel Animation';
                    // hide geoMarker
                    geoMarker.setStyle(null);
                    // just in case you pan somewhere else
                    map.getView().setCenter(center);
                    vectorLayer.on('postrender', moveFeature);
                    map.render();
                }
            }

            /**
             * @param {boolean} ended end of animation.
             */
            function stopAnimation(ended) {
                animating = false;
                startButton.textContent = 'Start Animation';

                // if animation cancelled set the marker at the beginning
                var coord = ended ? routeCoords[routeLength - 1] : routeCoords[0];
                var geometry = geoMarker.getGeometry();
                geometry.setCoordinates(coord);
                //remove listener
                vectorLayer.un('postrender', moveFeature);
            }

            startButton.addEventListener('click', startAnimation, false);

        },

        aa() {

        },

    },

    mounted() {

    },
}
</script>

<style>
#app {}

#map {
    width: 100%;
    height: 400px;
}

.ol-dragbox {
    background-color: rgba(255, 255, 255, 0.4);
    border-color: rgba(100, 150, 0, 1);
}
</style>
