<template>
<div class="wrap">
    <div class="map" id="map"></div>
    <MarkAdd class="MarkAdd" v-show="show_markadd" :show_markadd="show_markadd" @MarkAddVisibleEvent="Show_MarkAdd">
    </MarkAdd>

</div>
</template>

<script>
import MarkAdd from "../components/MarkAdd.vue" //添加标注弹窗
import olmap from "../components/olmap.vue"
export default {
    data() {
        return {
            checked1: false,
            show_markadd: false,
            map: '',
        };
    },
    components: {
        MarkAdd,
        olmap
    },

    methods: {
        Show_MarkAdd(visible) {
            this.show_markadd = visible;
        },
    },
    //显示标注
    viewMark() {

    },

};
</script>

<style>
/* 地图框 */
.map {
    position: absolute;
    width: 80%;
    left: 20%;
    top: 70px;
    bottom: 40px;

    /* background-color: #42B983; */
}

/* 显示标注按钮 */
.mark-show {
    position: absolute;
    top: 100px;
    right: 600px;
    width: 112px;
    background-color: white;
    border-radius: 5px;

}

/* 工具按钮 */
.buttons {
    position: absolute;
    top: 100px;
    right: 0;
    width: 500px;
}

.MarkAdd {
    position: absolute;
    top: 50%;
    left: 50%;
    margin: auto;
}
</style>
