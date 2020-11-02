<template>
    <div class="numberPad">
        <div class="output">{{output}}</div>
        <div class="buttons">
            <div class="number">
               <button class="btn" @click="inputContent">1</button>
               <button class="btn" @click="inputContent">2</button>
               <button class="btn" @click="inputContent">3</button>

            <button class="btn" @click="inputContent">4</button>
            <button class="btn" @click="inputContent">5</button>
            <button class="btn" @click="inputContent">6</button>

            <button class="btn" @click="inputContent">7</button>
            <button class="btn" @click="inputContent">8</button>
            <button class="btn" @click="inputContent">9</button>

            <button class="btn" @click="inputContent" id="zero">0</button>
            <button class="btn" @click="inputContent"><strong>.</strong></button>

            </div>

            <div class="tool">
            <button class="remove" id="btn1" @click="remove"><Icon name="退格"/></button>         
            <button class="clear" @click="clear">清空</button>
            <button class="ok" @click="ok" id="ok">OK</button>
            </div>
            
        </div>
    </div>
</template>

<script lang="ts">
    import Vue from 'vue'
    import {Component, Prop} from 'vue-property-decorator'

    @Component
    export default class NumberPad extends Vue {
        // output = '0'  /有.sync了，在Money里初始化数据
        @Prop(Number) readonly value!: number
        output = this.value.toString()

        inputContent(event: MouseEvent) {//传的的参数为点击事件的参数
            const button = (event.target as HTMLButtonElement)//强制将这个event.target改为按钮事件
            // eslint-disable-next-line @typescript-eslint/no-non-null-assertion
            const input = button.textContent! //!排除input为空的情况
            if (this.output.length === 16) {
                return
            }
            if (this.output === '0') {
                if ('0123456789'.indexOf(input) >= 0) {
                    this.output = input
                } else {
                    this.output += input
                }
                return
            }
            //如果output的.大于0或者输入· 就拜拜
            if (this.output.indexOf('.') >= 0 && input === '.') {
                return
            }
            this.output += input
        }

        remove() {
            if (this.output.length === 1) {
                this.output = '0'
            } else {
                this.output = this.output.slice(0, -1)
            }
        }

        clear() {
            this.output = '0'
        }

        ok() {
            const number=parseFloat(this.output)
            this.$emit('update:value',number)
            this.$emit('submit', number)
            this.output = '0'
        }
    }
</script>

<style lang="scss" scoped>
    @import "~@/assets/style/reset.scss";

    .numberPad {
        background: #9bcac2;
        padding: 14px 8px;
        border-top: 2px solid black;
        height: 48%;
        font-family: UD Digi Kyokasho NP-B,Consolas, monospace; 
        
        .output {
            @extend %clearFix;
            @extend %innerShadow;
            font-size: 24px;
            padding: 3px 16px;
            text-align: right;
            line-height: 32px;
            border: 2px solid black;
            border-radius: 10px;
            background-color: white;
        }
        
        .buttons {
            @extend %clearFix; 
            margin-top: 5px;
            display: flex;
            height: 100%;
            .number{
                width: 74%;
               & .btn{
                width: 30%;
                height: 17%;
                float: left;
                border: 2px solid black;
                background: white;
                border-radius:10px ;
                margin: 1%;
                font-size: 20px;
                &.btn:active {
                    background: rosybrown;
                }
                &#zero {
                    width: 31*2%;
                }
              
            }   
            

            }
        }
    }
.tool{
    width: 25%;
    > button{
      border: 2px solid black;
      background: white;
      border-radius:10px ;
      width: 100%;
      margin: 3%;
      font-size: 20px;
      &#ok {
          height: 37%;
          
      }
      &.clear,&.remove{
         height: 17%;
      }
    >.icon{
    width: 45px;
    height: 35px;   

    }
    
}
}

</style>