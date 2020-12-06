<template>
  <div class="about">
    <h1>This is an about page</h1>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
// import Vue from 'vue'
// import Component from 'vue-class-component'
// import {Vue, Component, Watch} from 'vue-property-decorator';

export default class About extends Vue {

  data: any = {
    '000': {
        id: 0,
        type: '00',
        children: [{
            id: 1,
            type: 'a'
          },{
            id: 2,
            type: 'b'
          },{
            id: 3,
            type: 'a',
            children: [{
              id: 6,
              type: 'b'
            },{
              id: 7,
              type: 'c',
              children: []
            },{
              id: 8,
              type: 'a',
              children: [{
                  id: 10,
                  type: 'a'
                },{
                  id: 11,
                  type: 'b'
                },{
                  id: 12,
                  type: 'a'
                }]
            },{
              id: 9,
              type: 'a'
            }]
          },{
            id: 4,
            type: 'b'
          },{
            id: 5,
            type: 'a'
          }]
      }
  }

  mounted() {
    let obj = this.getItem(9, this.data, '000');
    console.log('obj=====', obj);
    let num = this.getItemNum('a',this.data,'000');
    console.log('num====',num);
  }

  getItem(id: any, data: any, screenId: string) {
    let hasFound = false;// 表示是否有找到id值
    let result = null;
    let fn = (data: any) => {
      if (Array.isArray(data) && !hasFound) { // 判断是否是数组并且没有的情况下，
        for (let item of data) {
           if (item.id === id) { // 数据循环每个子项，并且判断子项下边是否有id值
            result = item; // 返回的结果等于每一项
            hasFound = true; // 并且找到id值
          } else if (item.children && item.children.length > 0) {
            fn(item.children); // 递归调用下边的子项
          }
        }
      }
    }
    fn(data[screenId].children); // 调用一下
    return result;
  }

  getItemNum(type: string, data: any, screenId: string) {

    let hasFound = false;// 表示是否有找到id值
    let num = 0;
    let fn = (data: any) => {
      if (Array.isArray(data)) { // 判断是否是数组并且没有的情况下，
        for (let item of data) {
           if (item.type === type) { // 数据循环每个子项，并且判断子项下边是否有id值
            num++; // 返回的结果等于每一项
          } 
          if (item.children && item.children.length > 0) {
            fn(item.children); // 递归调用下边的子项
          }
        }
      }
    }
    fn(data[screenId].children); // 调用一下
    return num;
  }

}
</script>