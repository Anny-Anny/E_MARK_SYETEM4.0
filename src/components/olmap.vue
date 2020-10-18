<template>
<div id="cc" :background-color="color1">
    <el-menu default-active="2" class="el-menu-vertical-demo" id="menu" :background-color="color1" text-color="#fff" active-text-color="#ffd04b">
        <el-submenu index="1">
            <template slot="title">
                <i class="iconfontxjw icxjwdaorushuju"></i>
                <span>导入数据</span>
            </template>
            <el-menu-item-group>
                <template slot="title">分组一</template>
                <el-menu-item index="1-1" @click="addVector">
                    <i class="font_family icon-shiliangtu"></i>
                    加载矢量地图
                </el-menu-item>
                <el-menu-item index="1-2" @click="addPoint"><i class="iconfont iconditubiaoji1"></i>加载点
                </el-menu-item>
                <el-menu-item index="1-3" @click="addLine"><i class="iconfont iconzhixian"></i>加载线

                </el-menu-item>
            </el-menu-item-group>
        </el-submenu>

        <el-submenu index="2">
            <template slot="title">
                <i class="iconfontxjw icxjwyaogantuxiangshujuku"></i>
                <span>图形渲染</span>
            </template>
            <el-menu-item-group>
                <template slot="title">滤波处理</template>
                <el-menu-item index="1-1" @click="filter()">
                    <i class="font_family icon-shiliangtu"></i>
                    <select id="kernel" name="kernel">
                        <option>none</option>
                        <option selected>sharpen</option>
                        <option value="sharpenless">sharpen less</option>
                        <option>blur</option>
                        <option>shadow</option>
                        <option>emboss</option>
                        <option value="edge">edge detect</option>
                    </select>
                </el-menu-item>
            </el-menu-item-group>
        </el-submenu>
        <el-submenu index="3">
            <template slot="title">
                <i class="iconfontxjw icxjwbiaoji"></i>
                <span>在线标记</span>
            </template>
            <el-menu-item-group>
                <template slot="title">基本标记</template>
                <el-menu-item index="1-1" @click="modifyDraw">
                    <form class="form-inline">
                        <label @click="modifyFDraw()">类型 &nbsp;</label>
                        <select id="modifyDrawtype">
                            <option value="Point">Point</option>
                            <option value="LineString">LineString</option>
                            <option value="Polygon">Polygon</option>
                            <option value="Circle">Circle</option>
                        </select>
                    </form>
                </el-menu-item>
            </el-menu-item-group>
            <el-menu-item-group title="自由绘制">
                <el-menu-item index="1-2">
                    <form class="form-inline">
                        <label @click="freeDraw()">类型 &nbsp;</label>
                        <select id="freeDrawtype">
                            <option value="LineString">LineString</option>
                            <option value="Polygon">Polygon</option>
                            <option value="Circle">Circle</option>
                            <option value="None">None</option>
                        </select>
                    </form>
                </el-menu-item>
            </el-menu-item-group>
            <el-menu-item-group title="图形标记">
                <el-menu-item index="1-3">
                    <form class="form-inline">
                        <label @click="markDraw()">类型 &nbsp;</label>
                        <select id="markDrawtype">
                            <option value="Circle">Circle</option>
                            <option value="Square">Square</option>
                            <option value="Box">Box</option>
                            <option value="Star">Star</option>
                            <option value="None">None</option>
                        </select>
                    </form>
                </el-menu-item>
            </el-menu-item-group>

            <el-menu-item-group title="要素编辑">
                <el-menu-item index="1-4">
                    <form class="form-inline">
                        <label @click="polylineEdit()">类型 &nbsp;</label>
                        <select id="mode">
                            <option value="modify">select&modify</option>
                            <option value="draw">draw</option>
                        </select>
                    </form>
                </el-menu-item>
            </el-menu-item-group>

        </el-submenu>

        <el-submenu index="4">
            <template slot="title">
                <i class="iconfontxjw icxjwceliang1"></i>
                <span>工具箱</span>
            </template>
            <el-menu-item-group title="测量工具">
                <el-menu-item index="1-3">
                    <form class="form-inline">
                        <label>测量类型 &nbsp;</label>
                        <select id="measuretype" @click="measure()">
                            <option value="length">Length (LineString)</option>
                            <option value="area">Area (Polygon)</option>
                        </select>
                    </form>
                    </form>
                </el-menu-item>
            </el-menu-item-group>

        </el-submenu>
        <el-submenu index="5">
            <template slot="title">
                <i class="iconfontxjw icxjwshujushuchu"></i>
                <span>数据输出</span>
            </template>
            <el-menu-item-group>
                <template slot="title">PNG格式</template>
                <el-menu-item index="1-1">
                    <i class="el-icon-picture-outline" @click="expPng()"></i>
                    <a id="export-png" class="btn btn-default"> Download PNG</a>
                    <a id="image-download" download="map.png"></a>
                </el-menu-item>

            </el-menu-item-group>
        </el-submenu>
        <el-submenu index="6">
            <template slot="title">
                <i class="iconfontxjw icxjwgexinghua"></i>
                <span>个性化设置</span>
            </template>

            <el-menu-item-group title="设置主题">
                <el-menu-item index="1-3">
                    <div class="block">
                        <span class="demonstration">有默认值</span>
                        <el-color-picker v-model="color1"></el-color-picker>
                    </div>
                </el-menu-item>
            </el-menu-item-group>
        </el-submenu>
    </el-menu>
    <div id="map" class="map-x"></div>
    <div class="buttons1">
        <!-- <el-checkbox v-model="checked1" label="显示标注" border></el-checkbox> -->
        <!-- 显示标注按钮 -->
        <el-button type="plain" v-show="false" size="mini">

        </el-button>
        <el-tooltip effect="dark" content="定位" placement="left">
            <el-button type="plain" @click="location()" size="mini">
                <i class="iconfont iconditubiaoji1"></i>
            </el-button>
        </el-tooltip>

        <el-tooltip effect="dark" content="添加所有控件" placement="left">
            <el-button type="plain" @click="addControl" size="mini">
                <i class="font_family icon-kongjian"></i>
            </el-button>
        </el-tooltip>

        <el-tooltip effect="dark" content="加载瓦片地图" placement="left">
            <el-button type="plain" @click="addTile()" size="mini">
                <i class="font_family icon-iWhereVisual-wapianditufuwu"></i>
            </el-button>
        </el-tooltip>

        <el-tooltip effect="dark" content="加载矢量地图" placement="left">
            <el-button type="plain" @click="addVector" size="mini">
                <i class="font_family icon-shiliangtu"></i>
            </el-button>
        </el-tooltip>

        <el-tooltip effect="dark" content="清空图层" placement="left">
            <el-button type="plain" @click="del()" size="mini">
                <i class="iconfontxjw icxjwshanchutuceng"></i>
            </el-button>
        </el-tooltip>
    </div>
    <div class="baseMap">
        <el-button @click="change_img" size="small" round>切换影像底图</el-button>
        <el-button @click="change_vec" size="small" round>切换街道底图</el-button>
        <el-button @click="change_ter" size="small" round>切换地形底图</el-button>
        <!-- <BaseMap :baseMap="baseMap" ref="baseMap"></BaseMap> -->
    </div>
    <div id="vectinfo" v-show="vector_show">
        <el-button size="small" round>
            <div id="info">No countries selected</div>
        </el-button>
    </div>
    <div id="locate" v-show=location_show>
        <div id="info" style="display: none;"></div>
        <label for="track">
            track position
            <input id="track" type="checkbox" />
        </label>
        <p>
            <br>
            position accuracy : <code id="accuracy"></code>&nbsp;&nbsp;<br>
            altitude : <code id="altitude"></code>&nbsp;&nbsp;<br>
            altitude accuracy : <code id="altitudeAccuracy"></code>&nbsp;&nbsp;<br>
            heading : <code id="heading"></code>&nbsp;&nbsp;<br>
            speed : <code id="speed"></code>
        </p>
    </div>
    <form id="point" v-show="point_show">
        <div id="status">Show impacts between <span class="min-year"></span> and <span class="max-year"></span></div>
        <label>Minimum year:</label>
        <input id="min-year" type="range" min="1850" max="2015" step="1" value="1850" />
        <label>Maximum year:</label>
        <input id="max-year" type="range" min="1850" max="2015" step="1" value="2015" />
    </form>
</div>
</template>

<!--ui的js文件-->

<script src="https://unpkg.com/elm-pep"></script>

<script src="https://api.mapbox.com/mapbox.js/plugins/arc.js/v0.1.0/arc.js"></script>

<script>
import "../view/Map.vue"
import 'ol/ol.css'
import {
    Map,
    View,
    Control
} from 'ol'
import {
    Tile as TileLayer,
    Vector as VectorLayer,
    Image,
    VectorImage as VectorImageLayer
} from 'ol/layer'
import {
    OSM,
    XYZ,
    Vector as VectorSource,
    ImageStatic
} from 'ol/source'

import Style from 'ol/style/Style'
import Icon from 'ol/style/Icon'
import Stroke from 'ol/style/Stroke'
import Circle from 'ol/style/Circle'
import Fill from 'ol/style/Fill'
import Polygon from 'ol/geom/Polygon'
import {
    MousePosition,
    ScaleLine,
    FullScreen
} from 'ol/control'
import Feature from 'ol/Feature'
import Point from 'ol/geom/Point';
import {
    transform,
    fromLonLat,
    toLonLat,
    Projection
} from 'ol/proj'
//加载点数据
import Stamen from 'ol/source/Stamen';
import WebGLPointsLayer from 'ol/layer/WebGLPoints';
//加载飞行数据

import LineString from 'ol/geom/LineString';
import {
    getVectorContext
} from 'ol/render';
//拖拽矢量数据+选择要素并高亮(成功)
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
    DragBox,
    Select
} from 'ol/interaction';
import {
    platformModifierKeyOnly
} from 'ol/events/condition';

//绘制标准图形

import {
    Modify,
    Snap
} from 'ol/interaction';
//自由绘制图形

import Draw, {
    createBox,
    createRegularPolygon,
} from 'ol/interaction/Draw';
import WKT from 'ol/format/WKT';
import {
    useGeographic
} from 'ol/proj';
//测量工具
import {
    getArea,
    getLength
} from 'ol/sphere';
import {
    unByKey
} from 'ol/Observable';
import Overlay from 'ol/Overlay';
//定位
import Geolocation from 'ol/Geolocation';

export default {
    // name: 'map',
    data() {
        return {
            map: null,
            color1: '#545c64',
            measureType: null,
            location_show: false,
            point_show: false,
            vector_show: false,

        }
    },
    mounted() {
        this.initmap();
    },

    methods: {
        initmap() {
            var map = new Map({
                layers: [
                    new TileLayer({
                        source: new XYZ({
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
            this.map = map;
        },
        del() {
            document.getElementById('map').innerHTML = '';
        },
        //清除info信息窗口
        delInfo() {
            this.location_show = false;
            this.point_show = false;
            this.vector_show = false;
        },
        addTile() {
            this.del();
            this.initmap();
        },
        //图像滤波
        filter() {
            this.del();
            var key = 'Get your own API key at https://www.maptiler.com/cloud/';
            var attributions =
                '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> ' +
                '<a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OpenStreetMap contributors</a>';

            var imagery = new TileLayer({
                source: new XYZ({
                    url: 'http://t3.tianditu.com/DataServer?T=img_w&x={x}&y={y}&l={z}&tk=d0cf74b31931aab68af181d23fa23d8d',
                    maxZoom: 20,
                    crossOrigin: '',
                }),
            });

            var map = new Map({
                layers: [imagery],
                target: 'map',
                view: new View({
                    center: fromLonLat([-120, 50]),
                    zoom: 6,
                }),
            });
            this.map = map;
            var kernels = {
                none: [0, 0, 0, 0, 1, 0, 0, 0, 0],
                sharpen: [0, -1, 0, -1, 5, -1, 0, -1, 0],
                sharpenless: [0, -1, 0, -1, 10, -1, 0, -1, 0],
                blur: [1, 1, 1, 1, 1, 1, 1, 1, 1],
                shadow: [1, 2, 1, 0, 1, 0, -1, -2, -1],
                emboss: [-2, 1, 0, -1, 1, 1, 0, 1, 2],
                edge: [0, 1, 0, 1, -4, 1, 0, 1, 0],
            };

            function normalize(kernel) {
                var len = kernel.length;
                var normal = new Array(len);
                var i,
                    sum = 0;
                for (i = 0; i < len; ++i) {
                    sum += kernel[i];
                }
                if (sum <= 0) {
                    normal.normalized = false;
                    sum = 1;
                } else {
                    normal.normalized = true;
                }
                for (i = 0; i < len; ++i) {
                    normal[i] = kernel[i] / sum;
                }
                return normal;
            }

            var select = document.getElementById('kernel');
            var selectedKernel = normalize(kernels[select.value]);

            /**
             * Update the kernel and re-render on change.
             */
            select.onchange = function () {
                selectedKernel = normalize(kernels[select.value]);
                map.render();
            };

            /**
             * Apply a filter on "postrender" events.
             */
            imagery.on('postrender', function (event) {
                convolve(event.context, selectedKernel);
            });

            /**
             * Apply a convolution kernel to canvas.  This works for any size kernel, but
             * performance starts degrading above 3 x 3.
             * @param {CanvasRenderingContext2D} context Canvas 2d context.
             * @param {Array<number>} kernel Kernel.
             */
            function convolve(context, kernel) {
                var canvas = context.canvas;
                var width = canvas.width;
                var height = canvas.height;

                var size = Math.sqrt(kernel.length);
                var half = Math.floor(size / 2);

                var inputData = context.getImageData(0, 0, width, height).data;

                var output = context.createImageData(width, height);
                var outputData = output.data;

                for (var pixelY = 0; pixelY < height; ++pixelY) {
                    var pixelsAbove = pixelY * width;
                    for (var pixelX = 0; pixelX < width; ++pixelX) {
                        var r = 0,
                            g = 0,
                            b = 0,
                            a = 0;
                        for (var kernelY = 0; kernelY < size; ++kernelY) {
                            for (var kernelX = 0; kernelX < size; ++kernelX) {
                                var weight = kernel[kernelY * size + kernelX];
                                var neighborY = Math.min(
                                    height - 1,
                                    Math.max(0, pixelY + kernelY - half)
                                );
                                var neighborX = Math.min(
                                    width - 1,
                                    Math.max(0, pixelX + kernelX - half)
                                );
                                var inputIndex = (neighborY * width + neighborX) * 4;
                                r += inputData[inputIndex] * weight;
                                g += inputData[inputIndex + 1] * weight;
                                b += inputData[inputIndex + 2] * weight;
                                a += inputData[inputIndex + 3] * weight;
                            }
                        }
                        var outputIndex = (pixelsAbove + pixelX) * 4;
                        outputData[outputIndex] = r;
                        outputData[outputIndex + 1] = g;
                        outputData[outputIndex + 2] = b;
                        outputData[outputIndex + 3] = kernel.normalized ? a : 255;
                    }
                }
                context.putImageData(output, 0, 0);
            }
        },
        //测量工具
        measure() {
            this.del();
            var raster = new TileLayer({
                source: new OSM(),
            });

            var source = new VectorSource();

            var vector = new VectorLayer({
                source: source,
                style: new Style({
                    fill: new Fill({
                        color: 'rgba(255, 255, 255, 0.2)',
                    }),
                    stroke: new Stroke({
                        color: '#ffcc33',
                        width: 2,
                    }),
                    image: new Circle({
                        radius: 7,
                        fill: new Fill({
                            color: '#ffcc33',
                        }),
                    }),
                }),
            });

            /**
             * Currently drawn feature.
             * @type {import("../src/ol/Feature.js").default}
             */
            var sketch;

            /**
             * The help tooltip element.
             * @type {HTMLElement}
             */
            var helpTooltipElement;

            /**
             * Overlay to show the help messages.
             * @type {Overlay}
             */
            var helpTooltip;

            /**
             * The measure tooltip element.
             * @type {HTMLElement}
             */
            var measureTooltipElement;

            /**
             * Overlay to show the measurement.
             * @type {Overlay}
             */
            var measureTooltip;

            /**
             * Message to show when the user is drawing a polygon.
             * @type {string}
             */
            var continuePolygonMsg = 'Click to continue drawing the polygon';

            /**
             * Message to show when the user is drawing a line.
             * @type {string}
             */
            var continueLineMsg = 'Click to continue drawing the line';

            /**
             * Handle pointer move.
             * @param {import("../src/ol/MapBrowserEvent").default} evt The event.
             */
            var pointerMoveHandler = function (evt) {
                if (evt.dragging) {
                    return;
                }
                /** @type {string} */
                var helpMsg = 'Click to start drawing';

                if (sketch) {
                    var geom = sketch.getGeometry();
                    if (geom instanceof Polygon) {
                        helpMsg = continuePolygonMsg;
                    } else if (geom instanceof LineString) {
                        helpMsg = continueLineMsg;
                    }
                }

                helpTooltipElement.innerHTML = helpMsg;
                helpTooltip.setPosition(evt.coordinate);

                helpTooltipElement.classList.remove('hidden');
            };

            var map = new Map({
                layers: [raster, vector],
                target: 'map',
                view: new View({
                    center: [-11000000, 4600000],
                    zoom: 15,
                }),
            });
            this.map = map;
            map.on('pointermove', pointerMoveHandler);

            map.getViewport().addEventListener('mouseout', function () {
                helpTooltipElement.classList.add('hidden');
            });

            var typeSelect = document.getElementById('measuretype');

            var draw; // global so we can remove it later

            /**
             * Format length output.
             * @param {LineString} line The line.
             * @return {string} The formatted length.
             */
            var formatLength = function (line) {
                var length = getLength(line);
                var output;
                if (length > 100) {
                    output = Math.round((length / 1000) * 100) / 100 + ' ' + 'km';
                } else {
                    output = Math.round(length * 100) / 100 + ' ' + 'm';
                }
                return output;
            };

            /**
             * Format area output.
             * @param {Polygon} polygon The polygon.
             * @return {string} Formatted area.
             */
            var formatArea = function (polygon) {
                var area = getArea(polygon);
                var output;
                if (area > 10000) {
                    output = Math.round((area / 1000000) * 100) / 100 + ' ' + 'km<sup>2</sup>';
                } else {
                    output = Math.round(area * 100) / 100 + ' ' + 'm<sup>2</sup>';
                }
                return output;
            };

            function addInteraction() {
                var type = typeSelect.value == 'area' ? 'Polygon' : 'LineString';
                draw = new Draw({
                    source: source,
                    type: type,
                    style: new Style({
                        fill: new Fill({
                            color: 'rgba(255, 255, 255, 0.2)',
                        }),
                        stroke: new Stroke({
                            color: 'rgba(0, 0, 0, 0.5)',
                            lineDash: [10, 10],
                            width: 2,
                        }),
                        image: new Circle({
                            radius: 5,
                            stroke: new Stroke({
                                color: 'rgba(0, 0, 0, 0.7)',
                            }),
                            fill: new Fill({
                                color: 'rgba(255, 255, 255, 0.2)',
                            }),
                        }),
                    }),
                });
                map.addInteraction(draw);

                createMeasureTooltip();
                createHelpTooltip();

                var listener;
                draw.on('drawstart', function (evt) {
                    // set sketch
                    sketch = evt.feature;

                    /** @type {import("../src/ol/coordinate.js").Coordinate|undefined} */
                    var tooltipCoord = evt.coordinate;

                    listener = sketch.getGeometry().on('change', function (evt) {
                        var geom = evt.target;
                        var output;
                        if (geom instanceof Polygon) {
                            output = formatArea(geom);
                            tooltipCoord = geom.getInteriorPoint().getCoordinates();
                        } else if (geom instanceof LineString) {
                            output = formatLength(geom);
                            tooltipCoord = geom.getLastCoordinate();
                        }
                        measureTooltipElement.innerHTML = output;
                        measureTooltip.setPosition(tooltipCoord);
                    });
                });

                draw.on('drawend', function () {
                    measureTooltipElement.className = 'ol-tooltip ol-tooltip-static';
                    measureTooltip.setOffset([0, -7]);
                    // unset sketch
                    sketch = null;
                    // unset tooltip so that a new one can be created
                    measureTooltipElement = null;
                    createMeasureTooltip();
                    unByKey(listener);
                });
            }

            /**
             * Creates a new help tooltip
             */
            function createHelpTooltip() {
                if (helpTooltipElement) {
                    helpTooltipElement.parentNode.removeChild(helpTooltipElement);
                }
                helpTooltipElement = document.createElement('div');
                helpTooltipElement.className = 'ol-tooltip hidden';
                helpTooltip = new Overlay({
                    element: helpTooltipElement,
                    offset: [15, 0],
                    positioning: 'center-left',
                });
                map.addOverlay(helpTooltip);
            }

            /**
             * Creates a new measure tooltip
             */
            function createMeasureTooltip() {
                if (measureTooltipElement) {
                    measureTooltipElement.parentNode.removeChild(measureTooltipElement);
                }
                measureTooltipElement = document.createElement('div');
                measureTooltipElement.className = 'ol-tooltip ol-tooltip-measure';
                measureTooltip = new Overlay({
                    element: measureTooltipElement,
                    offset: [0, -15],
                    positioning: 'bottom-center',
                });
                map.addOverlay(measureTooltip);
            }

            /**
             * Let user change the geometry type.
             */
            typeSelect.onchange = function () {
                map.removeInteraction(draw);
                addInteraction();
            };

            addInteraction();
        },
        //加载矢量地图
        addVector() {
            this.del();
            this.delInfo();
            this.vector_show = !this.vector_show;
            var dragAndDropInteraction = new DragAndDrop({
                formatConstructors: [GPX, GeoJSON, IGC, KML, TopoJSON],
            });

            //var key = 'Get your own API key at https://www.maptiler.com/cloud/';
            var attributions =
                '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a> ' +
                '<a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OpenStreetMap contributors</a>';
            // document.getElementById('map').empty();
            var map = new Map({
                interactions: defaultInteractions().extend([dragAndDropInteraction]),
                layers: [
                    new TileLayer({
                        source: new OSM(),
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
            this.map = map;
        },
        //加载点并渲染
        addPoint() {
            this.del();
            this.point_show = !this.point_show;
            var vectorSource = new VectorSource({
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

            // handle input values & events
            var minYearInput = document.getElementById('min-year');
            var maxYearInput = document.getElementById('max-year');

            function updateMinYear() {
                style.variables.minYear = parseInt(minYearInput.value);
                updateStatusText();
            }

            function updateMaxYear() {
                style.variables.maxYear = parseInt(maxYearInput.value);
                updateStatusText();
            }

            function updateStatusText() {
                var div = document.getElementById('status');
                div.querySelector('span.min-year').textContent = minYearInput.value;
                div.querySelector('span.max-year').textContent = maxYearInput.value;
            }

            minYearInput.addEventListener('input', updateMinYear);
            minYearInput.addEventListener('change', updateMinYear);
            maxYearInput.addEventListener('input', updateMaxYear);
            maxYearInput.addEventListener('change', updateMaxYear);
            updateStatusText();

            // load data
            var client = new XMLHttpRequest();
            client.open('GET', '../../static/meteorite_landings.csv');
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
                target: document.getElementById('map'),
                view: new View({
                    center: [0, 0],
                    zoom: 2,
                }),
            });
            this.map = map;
            // animate the map
            function animate() {
                map.render();
                window.requestAnimationFrame(animate);
            }
            animate();
        },
        //加载线并渲染（航线）
        addLine() {
            this.del();
            var tileLayer = new TileLayer({
                source: new Stamen({
                    layer: 'toner',
                }),
            });

            var map = new Map({
                layers: [tileLayer],
                target: 'map',
                view: new View({
                    center: [0, 0],
                    zoom: 2,
                }),
            });

            this.map = map;
            var style = new Style({
                stroke: new Stroke({
                    color: '#EAE911',
                    width: 2,
                }),
            });

            var flightsSource = new VectorSource({
                wrapX: false,
                attributions: 'Flight data by ' +
                    '<a href="http://openflights.org/data.html">OpenFlights</a>,',
                loader: function () {
                    var url = '../../static/flights.json';
                    fetch(url)
                        .then(function (response) {
                            return response.json();
                        })
                        .then(function (json) {
                            var flightsData = json.flights;
                            for (var i = 0; i < flightsData.length; i++) {
                                var flight = flightsData[i];
                                var from = flight[0];
                                var to = flight[1];

                                // create an arc circle between the two locations
                                var arcGenerator = new arc.GreatCircle({
                                    x: from[1],
                                    y: from[0]
                                }, {
                                    x: to[1],
                                    y: to[0]
                                });

                                var arcLine = arcGenerator.Arc(100, {
                                    offset: 10
                                });
                                if (arcLine.geometries.length === 1) {
                                    var line = new LineString(arcLine.geometries[0].coords);
                                    line.transform('EPSG:4326', 'EPSG:3857');

                                    var feature = new Feature({
                                        geometry: line,
                                        finished: false,
                                    });
                                    // add the feature with a delay so that the animation
                                    // for all features does not start at the same time
                                    addLater(feature, i * 50);
                                }
                            }
                            tileLayer.on('postrender', animateFlights);
                        });
                },
            });

            var flightsLayer = new VectorLayer({
                source: flightsSource,
                style: function (feature) {
                    // if the animation is still active for a feature, do not
                    // render the feature with the layer style
                    if (feature.get('finished')) {
                        return style;
                    } else {
                        return null;
                    }
                },
            });

            map.addLayer(flightsLayer);

            var pointsPerMs = 0.1;

            function animateFlights(event) {
                var vectorContext = getVectorContext(event);
                var frameState = event.frameState;
                vectorContext.setStyle(style);

                var features = flightsSource.getFeatures();
                for (var i = 0; i < features.length; i++) {
                    var feature = features[i];
                    if (!feature.get('finished')) {
                        // only draw the lines for which the animation has not finished yet
                        var coords = feature.getGeometry().getCoordinates();
                        var elapsedTime = frameState.time - feature.get('start');
                        var elapsedPoints = elapsedTime * pointsPerMs;

                        if (elapsedPoints >= coords.length) {
                            feature.set('finished', true);
                        }

                        var maxIndex = Math.min(elapsedPoints, coords.length);
                        var currentLine = new LineString(coords.slice(0, maxIndex));

                        // directly draw the line with the vector context
                        vectorContext.drawGeometry(currentLine);
                    }
                }
                // tell OpenLayers to continue the animation
                map.render();
            }

            function addLater(feature, timeout) {
                window.setTimeout(function () {
                    feature.set('start', new Date().getTime());
                    flightsSource.addFeature(feature);
                }, timeout);
            }
        },

        //加载静态地图
        addImgStatic() {

        },
        modifyDraw() {
            this.del();
            var raster = new TileLayer({
                source: new OSM(),
            });

            var source = new VectorSource();
            var vector = new VectorLayer({
                source: source,
                style: new Style({
                    fill: new Fill({
                        color: 'rgba(255, 255, 255, 0.2)',
                    }),
                    stroke: new Stroke({
                        color: '#ffcc33',
                        width: 2,
                    }),
                    image: new Circle({
                        radius: 7,
                        fill: new Fill({
                            color: '#ffcc33',
                        }),
                    }),
                }),
            });

            var map = new Map({
                layers: [raster, vector],
                target: 'map',
                view: new View({
                    center: [0, 0],
                    zoom: 2,
                }),
            });
            this.map = map;
            var modify = new Modify({
                source: source
            });
            map.addInteraction(modify);

            var draw, snap; // global so we can remove them later
            var typeSelect = document.getElementById('modifyDrawtype');

            function addInteractions() {
                draw = new Draw({
                    source: source,
                    type: typeSelect.value,
                });
                map.addInteraction(draw);
                snap = new Snap({
                    source: source
                });
                map.addInteraction(snap);
            }

            /**
             * Handle change event.
             */
            typeSelect.onchange = function () {
                map.removeInteraction(draw);
                map.removeInteraction(snap);
                addInteractions();
            };

            addInteractions();

        },
        markDraw() {
            this.del();
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
                    center: [0, 0],
                    zoom: 2,
                }),
            });
            this.map = map;
            var typeSelect = document.getElementById('markDrawtype');

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
        //自由绘制要素
        freeDraw() {
            this.del();
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
                    center: [0, 0],
                    zoom: 2,
                }),
            });
            this.map = map;
            var typeSelect = document.getElementById('freeDrawtype');

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
        polylineEdit() {
            this.del();
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
                    center: [0, 0],
                    zoom: 2,
                }),
            });
            this.map = map;
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
        //输出当前窗口地图为png
        expPng() {
            this.del();
            var raster = new TileLayer({
                source: new OSM(),
            });

            var format = new WKT();
            var feature = format.readFeature(
                'POLYGON((10.689697265625 -25.0927734375, 34.595947265625 ' +
                '-20.1708984375, 38.814697265625 -35.6396484375, 13.502197265625 ' +
                '-39.1552734375, 10.689697265625 -25.0927734375))'
            );
            feature.getGeometry().transform('EPSG:4326', 'EPSG:3857');

            var vector = new VectorLayer({
                source: new VectorSource({
                    features: [feature],
                }),
                opacity: 0.5,
            });

            var map = new Map({
                layers: [raster, vector],
                target: 'map',
                view: new View({
                    center: [0, 0],
                    zoom: 2,
                }),
            });

            var dims = {
                a0: [1189, 841],
                a1: [841, 594],
                a2: [594, 420],
                a3: [420, 297],
                a4: [297, 210],
                a5: [210, 148],
            };

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
        //添加控件
        addControl() {
            this.map.addControl(new MousePosition());
            this.map.addControl(new ScaleLine());
            this.map.addControl(new FullScreen());
        },
        //定位
        location() {
            this.point_show = false;
            this.location_show = !this.location_show;
            this.del();
            var view = new View({
                center: [0, 0],
                zoom: 2,
            });

            var map = new Map({
                layers: [
                    new TileLayer({
                        source: new OSM(),
                    })
                ],
                target: 'map',
                view: view,
            });
            this.map = map;
            var geolocation = new Geolocation({
                // enableHighAccuracy must be set to true to have the heading value.
                trackingOptions: {
                    enableHighAccuracy: true,
                },
                projection: view.getProjection(),
            });

            function el(id) {
                return document.getElementById(id);
            }

            el('track').addEventListener('change', function () {
                geolocation.setTracking(this.checked);
            });

            // update the HTML page when the position changes.
            geolocation.on('change', function () {
                el('accuracy').innerText = geolocation.getAccuracy() + ' [m]';
                el('altitude').innerText = geolocation.getAltitude() + ' [m]';
                el('altitudeAccuracy').innerText = geolocation.getAltitudeAccuracy() + ' [m]';
                el('heading').innerText = geolocation.getHeading() + ' [rad]';
                el('speed').innerText = geolocation.getSpeed() + ' [m/s]';
            });

            // handle geolocation error.
            geolocation.on('error', function (error) {
                var info = document.getElementById('info');
                info.innerHTML = error.message;
                info.style.display = '';
            });

            var accuracyFeature = new Feature();
            geolocation.on('change:accuracyGeometry', function () {
                accuracyFeature.setGeometry(geolocation.getAccuracyGeometry());
            });

            var positionFeature = new Feature();
            positionFeature.setStyle(
                new Style({
                    image: new Circle({
                        radius: 6,
                        fill: new Fill({
                            color: '#3399CC',
                        }),
                        stroke: new Stroke({
                            color: '#fff',
                            width: 2,
                        }),
                    }),
                })
            );

            geolocation.on('change:position', function () {
                var coordinates = geolocation.getPosition();
                positionFeature.setGeometry(coordinates ? new Point(coordinates) : null);
            });

            new VectorLayer({
                map: map,
                source: new VectorSource({
                    features: [accuracyFeature, positionFeature],
                }),
            });

        },
        //切换影像底图
        change_img() {
            var img = new TileLayer({
                source: new XYZ({
                    url: 'http://t3.tianditu.com/DataServer?T=img_w&x={x}&y={y}&l={z}&tk=d0cf74b31931aab68af181d23fa23d8d'
                })
            });
            this.map.addLayer(img)
            this.map.delete(map_vec);
            this.map.delete(map_cva);
            this.map.delete(map_ter);
            this.map.delete(map_cta);
        },
        change_vec() {
            var map_cva = new TileLayer({
                source: new XYZ({
                    url: "http://t3.tianditu.com/DataServer?T=cva_w&x={x}&y={y}&l={z}&tk=d0cf74b31931aab68af181d23fa23d8d"
                })
            });
            var map_vec = new TileLayer({
                source: new XYZ({
                    url: "http://t4.tianditu.com/DataServer?T=vec_w&x={x}&y={y}&l={z}&tk=d0cf74b31931aab68af181d23fa23d8d"
                })
            });

            this.map.addLayer(map_vec);
            this.map.addLayer(map_cva);

            this.map.delete(img);
            this.map.delete(map_ter);
            this.map.delete(map_cta);
            //console.log(this.map.getLayers());
        },
        change_ter() {
            var map_ter = new TileLayer({
                source: new XYZ({
                    url: "http://t4.tianditu.com/DataServer?T=ter_w&x={x}&y={y}&l={z}&tk=d0cf74b31931aab68af181d23fa23d8d"
                })
            });
            var map_cta = new TileLayer({
                source: new XYZ({
                    url: "http://t4.tianditu.com/DataServer?T=cva_w&x={x}&y={y}&l={z}&tk=d0cf74b31931aab68af181d23fa23d8d"
                })
            });
            this.map.addLayer(map_ter);
            this.map.addLayer(map_cta);

            this.map.delete(map_vec);
            this.map.delete(map_cva);
            this.map.delete(img);
        },
        viewMark() {

            console.log("1");
            this.pointLayer.setZIndex(1);
            this.lineLayer.setZIndex(2);
            this.polygonLayer.setZIndex(3);
            this.rectLayer.setZIndex(4);

        },
    },

    // components:{
    //    BaseMap,
    //  }
}
</script>

<style>
#mm {
    width: 100%;
    height: 400px;

}

#cc {
    width: 100%;
    height: 539px;
}

.map-x {
    position: absolute;
    width: 90%;
    /* z-index: -1; */
}

/* 工具按钮 */
.buttons1 {
    position: absolute;
    top: 110px;
    right: 10px;
    float: left;
    width: 60px;
}

.baseMap {
    position: absolute;
    bottom: 75px;
    left: 21%;
    float: left;
    width: 500px;
    opacity: 60%;
}

.item {
    /*    margin:400px */
}

.el-menu-vertical-demo:not(.el-menu--collapse) {
    display: inline-block;
    width: 20%;
    height: 558px;
    max-height: 558px;
    /*   min-height: 400px;*/
    /* z-index: -1;*/
}

#vectinfo {
    position: absolute;
    bottom: 115px;
    left: 21%;
    float: left;
    opacity: 80%;

}

#point {
    background-color: #545c64;
    color: blanchedalmond;
    padding: 3px;
    border-radius: 5px;
    position: absolute;
    bottom: 70px;
    right: 1%;
    float: right;
    width: 230px;
    opacity: 60%;

}

#locate {
    background-color: #545c64;
    color: blanchedalmond;
    padding: 3px;
    border-radius: 5px;
    position: absolute;
    bottom: 70px;
    right: 1%;
    float: right;
    width: 230px;
    opacity: 60%;
}

.ol-tooltip {
    position: relative;
    background: rgba(0, 0, 0, 0.5);
    border-radius: 4px;
    color: white;
    padding: 4px 8px;
    opacity: 0.7;
    white-space: nowrap;
    font-size: 12px;
}

.ol-tooltip-measure {
    opacity: 1;
    font-weight: bold;
}

.ol-tooltip-static {
    background-color: #ffcc33;
    color: black;
    border: 1px solid white;
}

.ol-tooltip-measure:before,
.ol-tooltip-static:before {
    border-top: 6px solid rgba(0, 0, 0, 0.5);
    border-right: 6px solid transparent;
    border-left: 6px solid transparent;
    content: "";
    position: absolute;
    bottom: -6px;
    margin-left: -7px;
    left: 50%;
}

.ol-tooltip-static:before {
    border-top-color: #ffcc33;
}
</style>
