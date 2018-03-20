<template>
  <div class="helloWordlKids">

    <div class="ShapesContainer">
      
      <div  class="col-md-5 mainContainer">
          <draggable class="list-group" element="ul" v-model="list" :options="dragOptions" :move="onMove" @start="isDragging=true" @end="isDragging=false">
            <transition-group type="transition" :name="'flip-list'">
              <li class="list-shape-item" v-for="element in list" :key="element.order">
                <i  @click=" element.fixed=! element.fixed" aria-hidden="true"></i>
                <div v-if="element.name == 'circle' ">
                  <div :class="element.name" v-bind:style="{ background:element.backgroundColor , width: element.width + 'px', height: element.height + 'px' }">
                  </div>
                </div>
                <div v-else-if="element.name == 'rectangle'" >
                  <div :class="element.name" v-bind:style="{ background:element.backgroundColor , width: (element.height)*2+ 'px', height: element.height + 'px'}">
                    <p style="text-align:center;">{{ element.text}}</p>
                  </div>
                </div>
                <div v-else="element.name == 'triangle'" >
                  <div :class="element.name" v-bind:style="{  color: element.backgroundColor ,  transform: 'rotate('+ element.rotate+'deg)' , 'border-left'  : element.height + 'px  solid transparent' , 'border-right'  : element.height  + 'px  solid transparent' , 'border-bottom'  : element.height*1.5 + 'px  solid'  }">
                  </div>
                </div>
              </li>
            </transition-group>
        </draggable>
      </div>

      <div class="col-md-5 mainContainer">
          <draggable class="list-group" element="ul" v-model="list" :options="dragOptions" :move="onMove" @start="isDragging=true" @end="isDragging=true">
            <transition-group type="transition" :name="'flip-list'">
              <li class="list-group-item" v-for="element in list" :key="element.order">
                <div v-if="editCanvas == false">
                  <select v-model="element.name" style="margin-top:50px;width:25%;float:left;">
                    <option value="circle">Circle</option>
                    <option value="triangle">Triangle</option>
                    <option value="rectangle">Rictangle</option>
                  </select>
                  <input type="range" @input="onInputValidator(element)" v-model="element.height" min="40" max="140" style="margin-right:20px;margin-left:20px;margin-top:50px;width:50%;float:left;">
                  <input type="color" v-model="element.backgroundColor" style="margin-top:50px;width:10%;float:left;">
                  <i class="el-icon-delete" @click="deleteShape(element)" style="margin-top:100px;float:left;"></i>
                </div>

                <div v-if="editCanvas == true">
                  <input v-if="element.name == 'rectangle'" type="text" v-model="element.text" style="margin-left:120px;margin-top:50px;width:50%;float:left;">
                  <input class="triangleInputRangeOne" v-if="element.name == 'triangle'" type="range" v-model="element.rotate" min="0" max="360" >
                  <input class="circleInputRangeOne" v-if="element.name == 'circle'" type="range" v-model="element.width" min="10" max="500" >
                  <input class="circleInputRangeTwo" v-if="element.name == 'circle'" type="range" v-model="element.height" min="10" max="160" >
                </div>

              </li>
            </transition-group>
            <el-button v-show="editCanvas == false" style="margin:10px;" class="btn" type="primary" icon="el-icon-edit" @click="changeNotEditable()"></el-button>
            <el-button v-show="editCanvas == true" style="margin:10px;" class="btn" type="primary" icon="el-icon-circle-check-outline" @click="changeNotEditable()"></el-button>
            <el-button style="margin:10px;" class="btn" type="primary" icon="el-icon-plus" @click="addNewShap()"></el-button>
        </draggable>
      </div>

    </div>
  </div>
  </div>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  name: 'hello',
  components: {
    draggable,
  },
  data () {
    return {
      color4 : null,
      editCanvas : false,
      list: [
      	{ name: 'circle', order : 1 , fixed : false , width : '60' , height :'60' , backgroundColor : '#D50000' , text : '' , borderLeft : 50 , borderRight : 50 , borderBottom : 100 , rotate : 0},
        { name: 'rectangle', order : 1 , fixed : false , width : '150' , height :'75' , backgroundColor : '#03A9F4' , text : '' , borderLeft : 50 , borderRight : 50 , borderBottom : 100 , rotate : 0},
        { name: 'triangle', order : 1 , fixed : false , width : '100' , height :'40' , backgroundColor : '#F3A9F4' , text : '' , borderLeft : 40 , borderRight : 50 , borderBottom : 100 , rotate : 0  }
      ],
      isDragging: false,
      delayedDragging:false
    }
  },
  computed: {
    dragOptions () {
      return  {
        group: 'description',
        ghostClass: 'ghost'
      };
    }
  },
  watch: {
    isDragging (newValue) {
      if (newValue){
        this.delayedDragging= true
        return
      }
      this.$nextTick( () =>{
           this.delayedDragging =false
      })
    }
  },
  methods:{
    onInputValidator(element) {
      element.width = element.height;
    },
    onMove ({relatedContext, draggedContext}) {
      const relatedElement = relatedContext.element;
      const draggedElement = draggedContext.element;
      return (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
    },
    addNewShap(){
      this.list.push({name: 'circle', order: 1 , fixed : false , width : '100' , height:'100' , backgroundColor : '#D50000' , text:'',  borderLeft : 50 , borderRight : 50 , borderBottom : 100 , rotate : 0})
    },
    deleteShape(element){
      const index = this.list.indexOf(element);
      this.list.splice(index, 1);

    },
    changeNotEditable(){
      console.log(this.editCanvas);
      if(this.editCanvas == true){
        this.editCanvas = false
      }
      else {
        this.editCanvas = true
      }
    }
  }
}
</script>

<style>
  .helloWordlKids{
    overflow: scroll;
  }
  .mainContainer{
    bottom: 0px;
    margin-left: 80px;
    border: 1px solid;
  }
  .flip-list-move {
    transition: transform 0.5s;
  }
  .btn{
    width:50px;
    height: 50px;
    float: right;
    padding-left: 18px !important;
    border-radius: 50% !important;
  }
  button:hover{
    background-color: #4CAF50;
    color: white !important;
  }
  .rectangle  {
    margin: auto;
    width: 200px;
    height: 100px;
  }
  .triangle {
    margin: auto;
    width: 0;
  	height: 0;
  }
  .circle {
    margin: auto;
  	width: 100px;
  	height: 100px;
  	background: red;
  	-moz-border-radius: 50%;
  	-webkit-border-radius: 50%;
  	border-radius: 50%;
  }
  .no-move {
    transition: transform 0s;
  }
  .ghost {
    opacity: .5;
    background: #C8EBFB;
  }
  .list-group {
    height: 20%;
  }
  .list-group {
    padding-left: 0;
    margin-bottom: 45px !important;
  }
  .list-group-item {
    border-top-left-radius: 16px !important;
    border-top-right-radius: 16px !important;
    border-bottom-right-radius: 16px !important;
    border-bottom-left-radius: 16px !important;
    margin:auto;
    cursor: move;
    margin:20px;
    height: 160px;
  }
  .list-shape-item{
    cursor: move;
    margin:20px;
    height: 160px;
  }
  ul{
    list-style-type: none;
  }
  .triangleInputRangeOne{
    margin-left:50px;
    margin-top:60px;
    width:400px !important;
    float:left;
  }
  .circleInputRangeOne{
    margin-left:80px;
    margin-top:60px;
    width:200px !important;
    float:left;
  }
  .circleInputRangeTwo{
    margin-right:50px;
    margin-top:60px;
    transform: rotate(90deg);
    width:120px !important;
    float:right;
  }
  .list-group-item i{
    cursor: pointer;
  }
</style>
