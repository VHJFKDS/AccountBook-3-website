<template>
    <ul class="tabs" :class="{[classPrefix+'-tabs']:classPrefix}">
        <li v-for="item in dataSource" :key="item.value"
            class="tabs-item" :style="{height:height}"
            :class="liClass(item)" @click="select(item)">
            {{item.text}}
        </li>
    </ul>
</template>

<script lang="ts">
    import Vue from 'vue'
    import {Component, Prop} from 'vue-property-decorator'

    type DataSourceItem = { text: string; value: string }

    @Component
    export default class Tabs extends Vue {
        @Prop({required: true, type: Array})
        dataSource!: DataSourceItem[]
        @Prop({type: String, default: '40px'})
        height!: string
        @Prop(String) readonly value!: string
        @Prop(String) classPrefix?: string

        liClass(item: DataSourceItem) {
            return {
                [this.classPrefix + '-tabs-item']: this.classPrefix,
                selected: item.value === this.value
            }
        }

        select(item: DataSourceItem) {
            this.$emit('update:value', item.value)
        }
    }

</script>

<style lang="scss" scoped>
    .tabs {
        background: #9bcac2;
        display: flex;
        text-align: center;
        font-size: 18px;
        align-items: center;
        justify-content: center;
        color: #333;
        position: relative;

        &-item { 
            width: 26%;
            
            transform: translate(0,10px);
            position: relative;

            &.selected::after { 
                content: '';
                display: flex;
                position: absolute;
                width: 90%;
                height: 0.1em;
                background: white;
                top: 69%;
                left: 5%;
            }
        }
    }
</style>