<template>
  <div class="main" @mousedown="mouseDown($event)" @mousemove="mouseMove($event)" @mouseup="mouseUp($event)">
          <div class="board"></div>
          <div v-for="(item,index) in pointsList" :key="index" :id="'point'+index" class="point" :style="{top:item.top+'px',left:item.left+'px'}" :draggable="false" @drag="drag($event,index)" @dragstart="dragStart($event,index)" @dragend="dragEnd($event,index)"></div>
          <div class="config">
                    <div>
                              <span>背景图</span> <input type="file" name="" id="" @change="getImg"> <img v-show="img" class="upImg" :src="img" alt="">
                    </div>
                    <div>
                              <span>背景色</span> <input type="color" name="" id="" @change="getBgc">
                    </div>
                    <div>
                              <span>添加点位</span> <button @click="addPoint">+</button>
                    </div>
          </div>
  </div>
</template>

<script>
export default {
          data(){
                    return {
                              img:'',
                              pointsList:[],
                              isFocus:false,
                    }
          },
          mounted(){
                    this.painter()
          },
          methods:{
                    mouseDown(e){
                              this.isFocus = true
                    },
                    mouseMove(e){
                              if(this.isFocus){
                                        this.pointsList.push({top:e.clientY-10,left:e.clientX-10})
                                        this.painter()
                              }
                    },
                    mouseUp(e){
                              this.isFocus = false
                    },
                    getImg(event){
                              this.img = URL.createObjectURL(event.target.files[0]);
                              let dom = document.querySelector('.board');
                              dom.style.backgroundImage = `url(${this.img})`;
                    },
                    getBgc(event){
                              let dom = document.querySelector('.board');
                              dom.style.backgroundColor = event.target.value;
                    },
                    dragStart(e){

                    },
                    drag(e,index){
                              let point = document.getElementById('point'+index);
                              point.style.left = e.clientX + 'px';
                              point.style.top = e.clientY + 'px';
                              this.painter()
                    },
                    dragEnd(){

                    },
                    addPoint(){
                              this.pointsList.push({})
                              setTimeout(() => {
                                        this.painter()   
                              }, 200);
                              
                    },
                    painter(){
                              let dom = document.querySelector('.board');
                              let pointsDom = document.querySelectorAll('.point');
                              let clipStr = ''
                              for(let i=0;i<pointsDom.length;i++){
                                        clipStr += `${pointsDom[i].getBoundingClientRect().left}px ${pointsDom[i].getBoundingClientRect().top}px,`; 
                              }
                              dom.style.clipPath= `polygon(${clipStr.slice(0, -1)})`;
                    },
          }
}
</script>

<style lang="less">
          .main{
                    display: flex;
                    height: 98vh;
                    position: relative;
                    .board{
                              flex:2;
                              width: 100%;
                              height: 100%;
                              background-color: grey;
                              object-fit: contain;
                              background-size: contain;
                              background-repeat: no-repeat;
                              
                              
                    }
                    .point{
                                        cursor: pointer;
                                        position: absolute;
                                        width: 1px;
                                        height: 1px;
                                        background-color: rgba(0, 0, 0, 0);
                                        
                              }
                    .config{
                              flex:1;
                              .upImg{
                                        width: 100px;
                                        height: 100px;
                                        object-fit: contain;
                              }
                    }
          }
</style>