<template>
  <div>
    <ul>
      <li>在js中获取一个ref的值都要加.value,除了ref里面嵌套的ref或者被reactive嵌套的ref</li>
      <li>在html只要返回时没有被嵌套在对象中，读取ref就不需要.value,否则就需要</li>
      <li>在js中访问由ref组成的数组里面的元素都需要.value</li>
      <li>在html中访问由ref组成的数组里面的元素除了简单值都需要.value</li>
    </ul>
    <div style="margin:10px 0;">
      <h1>普通值</h1>
      <div>
        <h2>测试普通值是否拥有响应</h2>
        <p><i>普通值没有响应</i></p>
        <span>{{ demo1A }}</span>
        <!-- 在模板中ref读取值不需要.value -->
        <button @click="demo1A = 'b'">更新demo1.a</button>
      </div>
      <hr/>
    </div>
    <div style="margin:10px 0;">
      <h1>普通对象</h1>
      <div>
        <h2>测试普通对象是否拥有响应</h2>
        <p><i>普通对象没有响应</i></p>
        <span>{{ demo1.a }}</span>
        <span>{{ demo1.b.a }}</span>
        <!-- 在模板中ref读取值不需要.value -->
        <button @click="demo1.a ++">更新demo1.a</button>
      </div>
      <hr/>
    </div>
    <div style="margin:10px 0;">
      <h1>普通数组</h1>
      <div>
        <h2>测试普通数组里面的元素是普通值是否拥有响应</h2>
        <p><i>普通数组没有响应</i></p>
        <span>{{ list[0] }}</span>
        <span>{{ list[1] }}</span>
        <!-- 在模板中ref读取值不需要.value -->
        <button @click="list[0].value++">list[0].value++</button>
        <button @click="list[1].value++">list[1].value++</button>
      </div>
      <div>
        <h2>测试普通数组里面是ref值是否拥有响应</h2>
        <p><i>普通数组里面元素是ref拥有响应</i></p>
        <span v-for="item in demo1B">{{ item }}</span>
        <!-- 在模板中ref读取值不需要.value -->
        <button @click="demo1B = [5, 6, 7]">更新demo1.a</button>
      </div>
      <hr/>
    </div>
    <div style="margin:10px 0;">
      <h1>单个ref</h1>
      <div>
        <h2>ref 把原始值处理为响应式</h2>
        <p><i>正常读取并更新了</i></p>
        <span>{{ count }}</span>
        <span>{{ nested.count.value }}</span>
        <!-- 在模板中ref读取值不需要.value -->
        <button @click="count ++">使ref值增加</button>
        <!-- 但是嵌套的ref就需要.value -->
        <button @click="nested.count.value ++">嵌套的ref值增加</button>
      </div>
      <div>
        <h2>原始值嵌套ref</h2>
        <p><i>正常读取并更新了 要加上value</i></p>
        <span>{{ nested.count.value }}</span>
        <!-- 但是嵌套的ref就需要.value -->
        <button @click="nested.count.value ++">嵌套的ref值增加</button>
      </div>
      <div>
        <h2>ref 数组响应式 赋值整个数组</h2>
        <p><i>正常读取并更新了</i></p>
        <span v-for="item in counts">{{ item }}</span>
        <button @click="counts = [1, 2, 3]">赋值数组</button>
        <button @click="counts.push(4)">增加数组项</button>
      </div>
      <div>
        <h2>数组嵌套ref</h2>
        <p><i>嵌套的情况要加上value 要加上value</i></p>
        <span v-for="item in nested.counts.value">{{ item }}</span>
        <button @click="nested.counts.value = [1, 2, 3]">赋值数组</button>
        <!-- 但是嵌套的ref就需要.value -->
        <button @click="nested.counts.value.push(4)">嵌套的ref值增加</button>
      </div>
      <div>
        <h2>ref 数组的元素是ref</h2>
        <p><i>元素是ref时，在html中即可加.value又可以不加 但在Js中要加.value来获取值</i></p>
        <span v-for="item in countRefs">{{ item }}</span>
        <p>{{ countRefs[0] }} - {{ countRefs[0].value }}</p>
        <p>{{ countRefs[1] }} - {{ countRefs[1].value }}</p>
        <p>{{ countRefs[2] }} - {{ countRefs[2].value }}</p>
        <button @click="countRefs[0].value++">countRefs[0].value</button>
      </div>
      <div>
        <h2>ref 数组的最后一个元素是ref数组对象</h2>
        <p><i>数组里面的ref的值都需要用.value来获取</i></p>
        <span v-for="item in countRefs[3].value">{{ item }}</span>
        <button @click="countRefs[3].value[0]++">countRefs[3].value[0]++</button>
      </div>
      <div>
        <h2>ref 数组的最后一个元素是ref对象</h2>
        <p><i>数组里面的ref的值都需要用.value来获取</i></p>
        <span>{{ countRefs[4].value.count2 }}</span>
        <button @click="countRefs[4].value.count2++">countRefs[4].value.count2++</button>
      </div>
      <div>
        <h2>ref 数组对象响应式</h2>
        <p><i>正常读取并更新了</i></p>
        <span>{{ refObjs[0].a }}</span>
        <span>{{ refObjs[0].b }}</span>
        <button @click="refObjs[0].a = 3">赋值数组的对象值</button>
        <button @click="refObjs[0].a++">更新数组的对象值</button>
        <button @click="refObjs[0].b = 1">赋值state.objs[0].b</button>
        <button @click="refObjs[0].b++">赋值state.objs[0].b</button>
      </div>
      <div>
        <h2>ref 嵌套数组对象响应式</h2>
        <p><i>嵌套的情况要加上value 正常读取并更新了</i></p>
        <span>{{ nested.refObjs.value[0].a }}</span>
        <span>{{ nested.refObjs.value[0].b }}</span>
        <button @click="nested.refObjs.value[0].a = 3">赋值数组的对象值</button>
        <button @click="nested.refObjs.value[0].a++">更新数组的对象值</button>
        <button @click="nested.refObjs.value[0].b = 1">赋值state.objs[0].b</button>
        <button @click="nested.refObjs.value[0].b++">赋值state.objs[0].b</button>
      </div>
      <div>
        <h2>对象值ref</h2>
        <p><i>正常访问</i></p>
        <span>{{ obj.a }}</span>
        <span>{{ obj.b }}</span>
        <!-- 但是嵌套的ref就需要.value -->
        <button @click="obj.a ++">增加obj.a</button>
        <button @click="obj.a = 2">赋值obj.a</button>
        <button @click="obj.b ++">增加obj.b</button>
        <button @click="obj.b = 3">赋值obj.b</button>
      </div>
      <div>
        <h2>嵌套对象值ref</h2>
        <p><i>加上value 正常访问</i></p>
        <span>{{ nested.obj.value.a }}</span>
        <span>{{ nested.obj.value.b }}</span>
        <!-- 但是嵌套的ref就需要.value -->
        <button @click="nested.obj.value.a ++">增加obj.a</button>
        <button @click="nested.obj.value.a = 2">赋值obj.a</button>
        <button @click="nested.obj.value.b ++">增加obj.b</button>
        <button @click="nested.obj.value.b = 3">赋值obj.b</button>
      </div>
      <div>
        <h2>ref的对象里面一个属性是ref</h2>
        <p><i>正常访问</i></p>
        <span>{{ obj.count2 }}</span>
        <!-- 但是嵌套的ref就需要.value -->
        <button @click="obj.count2 ++">增加obj.count2</button>
      </div>
      <div>
        <h2>ref的对象里面一个属性是ref数组，里面的属性是ref</h2>
        <p><i>带.value不带.value都可以正常访问，但是操作的时候要加.value</i></p>
        <span>{{ obj.countRefs[0] }}</span>
        <!-- 但是嵌套的ref就需要.value -->
        <button @click="obj.countRefs[0].value ++">增加obj.countRefs[0]</button>
      </div>
      <div>
        <h2>ref的对象里面一个属性是普通对象里面有一个ref属性</h2>
        <p><i>正常访问</i></p>
        <span>{{ obj.subObj.count3 }}</span>
        <span>{{ obj.subObj.a }}</span>
        <!-- 但是嵌套的ref就需要.value -->
        <button @click="obj.subObj.count3++">增加obj.count3</button>
        <button @click="obj.subObj.a++">obj.subObj.a</button>
      </div>
      <div>
        <h2>ref的对象里面一个属性是ref对象</h2>
        <p><i>正常访问</i></p>
        <span>{{ obj.obj2.count2 }}</span>
        <span>{{ obj.obj2.subObj.count3 }}</span>
        <!-- 但是嵌套的ref就需要.value -->
        <button @click="obj.obj2.subObj.count3++">增加obj.obj2.subObj.count3</button>
      </div>
      <hr/>
    </div>
    <div style="margin:10px 0;">
      <h1>单个reactive返回一个对象</h1>
      <div>
        <h2>读取reactive浅层数据</h2>
        <p><i>正常读取并更新了</i></p>
        <span>{{ state.count }}</span>
        <!-- 不行 没有值 -->
        <span>{{ state.count.value }}</span>
        <button @click="state.count ++">Increment count</button>
        <!-- 不行 不会触发 -->
        <button @click="state.count.value ++">Nested Increment count</button>
      </div>
      <div>
        <h2>读取reactive深层对象数据</h2>
        <p><i>正常读取并更新了</i></p>
        <span>{{ state.nested.count }}</span>
        <button @click="state.nested.count ++">更新深层</button>
      </div>
      <div>
        <h2>读取reactive浅层层数组数据</h2>
        <p><i>正常读取并更新了</i></p>
        <span v-for="item in state.counts">{{ item }}</span>
        <button @click="state.counts = [1, 2, 3, 4]">赋值state.counts</button>
        <button @click="state.counts.push(4)">添加元素state.counts</button>
      </div>
      <div>
        <h2>读取reactive深层数组数据</h2>
        <p><i>正常读取并更新了</i></p>
        <span v-for="item in state.nested.counts">{{ item }}</span>
        <button @click="state.nested.counts = [1, 2, 3, 4]">赋值state.counts</button>
        <button @click="state.nested.counts.push(4)">添加元素state.counts</button>
      </div>
      <div>
        <h2>读取reactive浅层数组对象数据</h2>
        <p><i>正常读取并更新了</i></p>
        <span>{{ state.objs[0].a }}</span>
        <span>{{ state.objs[0].b }}</span>
        <button @click="state.objs[0].a++">赋值state.objs[0].a</button>
        <button @click="state.objs[0].b = 1">赋值state.objs[0].b</button>
        <button @click="state.objs[0].b++">赋值state.objs[0].b</button>
      </div>
      <div>
        <h2>读取reactive深层ref对象数据</h2>
        <!-- <p><i>正常读取并更新了</i></p> -->
        <span>{{ state.obj.a }}</span>
        <span>{{ state.obj.count2 }}</span>
        <span>{{ state.obj.obj2.count2 }}</span>
        <span>{{ state.obj.obj2.subObj.count3 }}</span>
        <span>{{ state.obj.countRefs[4].value.count2 }}</span>
        <button @click="state.obj.a++">赋值state.objs[0].a</button>
        <button @click="state.obj.count2++">赋值state.objs[0].a</button>
        <button @click="state.obj.obj2.count2++">赋值state.objs[0].a</button>
        <button @click="state.obj.obj2.subObj.count3++">赋值state.objs[0].a</button>
        <button @click="state.obj.countRefs[4].value.count2++">赋值state.objs[0].a</button>
      </div>
      <hr/>
    </div>
  </div>
</template>

<script>
  import { ref, reactive  } from 'vue'
  export default {
    setup() {
      // ref 读取值需要使用.value
      const count = ref(0)
      const count2 = ref(20)
      const count3 = ref(30)
      const counts = ref([0, 1, 2])
      const refObjs = ref([{a:1}])
      const obj2 = ref({
        count2,
        subObj: {
          count3
        }
      })
      const countRefs = ref([count, count2, count3, counts, obj2])
      const list = [count, count2, count3, counts, countRefs]
      const obj = ref({
        a: 1,
        count2,
        countRefs,
        subObj: {
          a: 1,
          count3
        },
        obj2
      })
      // 此时读取也不用.value
      console.log('obj.count2', count.value++, obj.value.count2)
      console.log('obj.subObj.count3', obj.value.subObj.count3)
      console.log('obj.obj2', obj.value.obj2.count2)
      // 数组中的ref需要.value
      console.log('countRefs[0]', countRefs.value[0].value)
      console.log('countRefs[3]', countRefs.value[3].value[0])
      // 对象中的属性为countRefs
      console.log('obj.countRefs[0]', obj.value.countRefs[0].value)

      // 响应式状态
      const state = reactive({
        count: 0,
        counts: [1, 2, 3],
        countRefs,
        objs: [{a:1}],
        obj,
        nested: {
          count: 1,
          counts: [1, 2, 3, 4],
          countRefs,
          objs: [{a:1}],
          obj
        }
      })

      return {
        // ref
        count,
        counts,
        list,
        countRefs,
        refObjs,
        obj,
        nested: {
          count,
          counts,
          list,
          countRefs,
          refObjs,
          obj
        },
        // reactive
        state,

        demo1A: 'a',
        demo1B: [1,2, 3],
        demo1: {
          a: 1,
          b: {
            a: 1
          }
        }
      }
    }
  }
</script>