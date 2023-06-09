<script lang="ts" setup>
import {onMounted, ref} from "vue";

let dragShow = ref(false)
// let {
//     deg = 0,
//     width = 'auto',
//     height = 'auto',
//     left = 0,
//     top = 0,
// } = defineProps<{
//     deg: number,
//     width: number | string,
//     height: number | string,
//     left: number,
//     top: number,
// }>()
  let deg = 0
  let width = 'auto'
  let height = 'auto'
  let left = ref(0)
  let top = ref(0)
onMounted(()=>{
//     修改旋转角度
//     避免Uncaught (in promise) TypeError: Assignment to constant variable.

})

// 鼠标在 down 执行的操作
function dragmousedown(e: MouseEvent) {
    dragShow.value =! dragShow.value
}


function dragmousemove(e: MouseEvent) {
    if (!dragShow.value) return;
    let {width: eleWidth, height: eleHeight} = e.target.getBoundingClientRect()
    let {offsetX: x, offsetY: y} = e
    if (x <= 10 && y <= 10) {
        console.log('左上')
    } else if (x >= Math.floor(eleWidth) - 10 && y <= 10) {
        console.log('右上')
    } else if (x <= 10 && y >= Math.floor(eleHeight) - 10) {
        console.log('左下')
    } else if (x >= Math.floor(eleWidth) - 10 && y >= Math.floor(eleHeight) - 10) {
        console.log('右下')
    } else { // 当在中间 可以拖动
     // 获取鼠标移动的距离
        let {movementX, movementY} = e
        left.value += movementX
        top.value += movementY
    }
}

function dragmouseup() {
}

</script>


<template>
    <div
            @mousedown="dragmousedown"
            @mousemove="dragmousemove"
            @mouseup="dragmouseup"
            :style="{
          rotate:deg+'deg',
          width:width == 'auto' ? 'auto' : width + 'px',
          height:height == 'auto' ? 'auto' : height + 'px',
          left:left + 'px',
          top:top + 'px',
      }"
            :class="{
          drag: dragShow
      }"
    >
        <slot></slot>
    </div>
</template>


<style scoped>
.drag {
    width: auto;
    height: auto;
    /*虚线*/
    outline: 1px dashed white;
    padding: 10px;
    position: relative;
}

* {
    cursor: pointer;
    user-select: none;

}
</style>
