<template>
    <div class="tags">
        <ul class="current">
            <li v-for="tag in tagList" :key="tag.id"
                :class="{selected: selectedTags.indexOf(tag)>=0
                &&selectedTags.indexOf(tag)<1}"
                @click="toggle(tag)">
                 <div class="tmd" v-if="tag.name">
                    <Icon :name="tag.name" class="tagsIcon"/>
                </div>
                <span class="tmd2">{{tag.name}}</span>
            </li>
            
        </ul>
    </div>
</template>

<script lang="ts">
    import {Component} from 'vue-property-decorator'
    import {mixins} from 'vue-class-component'
    import TagHelper from '@/mixins/TagHelper'

    @Component({})
    export default class Tags extends mixins(TagHelper) {
        selectedTags: string[] = [];

        get tagList() {
            return this.$store.state.tagList
        }

        created() {
            this.$store.commit('fetchTags')
        }
        toggle(tag: string) {
            const index = this.selectedTags.indexOf(tag)
            if (index >= 0) {
                this.selectedTags.splice(index, 1)
            }
            else{
                this.selectedTags.splice(index, 1)
                this.selectedTags.push(tag)//点击后执行这个函数，当选中后，tag会被push到selectedTags上
            }
            this.$emit('update:value', this.selectedTags)
        }
    }
</script>

<style lang="scss" scoped>

    .tags {
     font-size: 14px;
     padding: 12px 5px;
     height: 37%;
     overflow: auto;
        > .current {
           display: flex;
           flex-wrap: wrap;
           flex-direction: column;
           height: 100%;
           text-align: center;
            > li {
                width:20%;
                border-radius: 12px;
                padding: 1px 8px;
                margin: 2px 8px;
                border: 2px solid #f5f5f5;
             &.selected{
                background: #A0DECF;
                border: 2px solid #9BCAC2;
             }
               
            }
        }

        
    }
.tmd{
   .tagsIcon{
    width: 40px;
    height: 40px;
}
}

    

</style>