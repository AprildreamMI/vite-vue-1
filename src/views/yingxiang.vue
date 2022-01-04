<template>
  <div>
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
        <h2>测试普通数组是否拥有响应</h2>
        <p><i>普通数组没有响应</i></p>
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
        <button @click="count ++">Increment count</button>
        <!-- 但是嵌套的ref就需要.value -->
        <button @click="nested.count.value ++">Nested Increment count</button>
      </div>
      <div>
        <h2>ref 数组响应式 赋值整个数组</h2>
        <p><i>正常读取并更新了</i></p>
        <span v-for="item in counts">{{ item }}</span>
        <button @click="counts = [1, 2, 3]">赋值数组</button>
        <button @click="counts.push(4)">增加数组项</button>
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
      const counts = ref([0, 1, 2])
      const refObjs = ref([{a:1}])

      // 响应式状态
      const state = reactive({
        count: 0,
        counts: [1, 2, 3],
        objs: [{a:1}],
        nested: {
          count: 1,
          counts: [1, 2, 3, 4],
          objs: [{a:1}],
        }
      })
      return {
        count,
        counts,
        refObjs,

        nested: {
          refObjs,
          count,
          counts
        },
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