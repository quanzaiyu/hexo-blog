---
title: Vue的简单上传插件vue-easy-uploader
categories:
  - Vue
tags:
  - Vue
---

# 前言

这段时间赶项目，需要用到多文件上传，用Vue进行前端项目开发。在网上找了不少插件，都不是十分满意，有的使用起来繁琐，有的不能适应本项目。就打算自己折腾一下，写一个Vue的上传插件，一劳永逸，以后可以直接使用。

目前`vue-easy-uploader`已上传到`GitHub`和`NPM`，使用起来方便简单，不需要繁琐的配置即可投入生产，不过需要后端配合，实现上传接口。

本项目GitHub地址: https://github.com/quanzaiyu/vue-easy-uploader

本项目NPM地址: https://www.npmjs.com/package/vue-easy-uploader

详细的使用方法都在仓库Readme中，就不赘述，这里谈下本插件的设计开发思路。



# 插件介绍

`vue-easy-uploader`是一个多图上传插件。主要特性包括:

- 多文件上传
- 上传图片预览
- 上传状态监测
- 删除指定图片
- 清空图片
- 重新上传

后期版本迭代将不限于图片，往通用文件上传进行改进。

先看看上传插件使用时候的效果图:

![](https://raw.githubusercontent.com/quanzaiyu/vue-easy-uploader/master/static/05.png) ![](https://raw.githubusercontent.com/quanzaiyu/vue-easy-uploader/master/static/06.png)



# 目录结构

![](http://xiaoyulive.oss-cn-beijing.aliyuncs.com/imgs/0006.png)

```
index.js # 主入口文件
store.js # 状态管理
uploader.vue # 上传组件
```



# 文件解析

## index.js

```js
import uploader from './uploader'
import store from './store'

let plugin = {}

plugin.install = function (_Vue, _store) {
  _Vue.component('uploader', uploader)
  _store.registerModule('imgstore', store)
}

export default plugin
```

这是插件的主入口文件，注册了全局的上传组件和状态管理，使用时只需要在项目入口文件(一般是`main.js`)中加入以下代码即可引入此插件:

```js
import Vue from 'vue'
import Vuex from 'vuex'
import uploader from 'vue-easy-uploader'
 
let store = new Vuex.Store({})
Vue.use(uploader, store)
```



## store.js

此文件为状态管理配置文件，主要包含三个`state`:

```bash
img_upload_cache # 上传文件缓存
img_paths # 上传状态，包括 ready selected uploading finished
img_status # 上传后的路径反馈数组(数据结构为Set集合)
```

针对每个`state`都有自己的`mutation`，用于改变`state`，规范上`mutation`都需要使用大写字母加下划线的形式，本人习惯使用小写字母，不过都不是原则上的问题。

最重要的一个`state`是`img_status`，用于监视图片上传的状态。包括以下几个状态:

```bash
ready # 上传开始前的准备状态 
selected # 已选择上传文件 
uploading # 开始上传 
finished # 上传完毕 
```

在组件中可以通过改变上传状态实现文件的上传，同时也可以监听上传状态的变化而执行回调。如:

```js
methods: {
  upload () {
    this.$store.commit('set_img_status', 'uploading')
  },
  submit () {
    // some code
  }
}
computed: {
  ...mapState({
    imgStatus: state => state.imgstore.img_status
  })
},
watch: {
  imgStatus () {
    if (this.imgStatus === 'finished') {
      this.submit()
    }
  }
}
```

上述代码中，使用`upload`方法更新了上传状态，让图片开始执行上传操作，使用`watch`进行上传状态的监视，当上传完成(`img_status`状态变为`finished`)，执行回调函数`submit`。

源文件如下:

```js
// Created by quanzaiyu on 2017/10/25 0025.
var state = {
  img_upload_cache: [],
  img_paths: [],
  img_status: 'ready' // 上传状态 ready selected uploading finished
}

const actions = {}

const getters = {}

const mutations = {
  set_img_upload_cache (state, arg) {
    state.img_upload_cache = arg
  },
  set_img_paths (state, arg) {
    state.img_paths = arg
  },
  set_img_status (state, arg) {
    state.img_status = arg
  }
}

export default {
  state,
  mutations,
  actions,
  getters
}
```



## uploader.vue

先看源代码(为了节省空间，未贴出`style`部分的代码):

```vue
<template>
  <div class="imgUploader">
    <div class="file-list">
      <section
        v-for="(file, index) in imgStore" :key="index"
        class="file-item draggable-item"
      >
        <img :src="file.src" alt="" ondragstart="return false;">
        <span class="file-remove" @click="remove(index)">+</span>
      </section>
      <section class="file-item" v-if="imgStatus !== 'finished'">
        <div class="add">
          <span>+</span>
          <input type="file" pictype='30010003' multiple
            data-role="none" accept="image/*"
            @change="selectImgs"
            ref="file"
          >
        </div>
      </section>
    </div>
    <div class="uploadBtn">
      <section>
        <span v-if="imgStore.length > 0" class="empty"
          @click="empty">
            {{imgStatus === 'finished' ? '重新上传' : '清空'}}
        </span>
      </section>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
  props: ['url'],
  data () {
    return {
      files: [], // 文件缓存
      index: 0 // 序列号
    }
  },
  computed: {
    ...mapState({
      imgStore: state => state.imgstore.img_upload_cache,
      imgPaths: state => state.imgstore.img_paths,
      imgStatus: state => state.imgstore.img_status
    })
  },
  methods: {
    // 选择图片
    selectImgs () { # ①
      let fileList = this.$refs.file.files
      for (let i = 0; i < fileList.length; i++) {
        // 文件过滤
        if (fileList[i].name.match(/.jpg|.gif|.png|.bmp/i)) { 
          let item = {
            key: this.index++,
            name: fileList[i].name,
            size: fileList[i].size,
            file: fileList[i]
          }
          // 将图片文件转成BASE64格式
          let reader = new FileReader()  # ②
          reader.onload = (e) => {
            this.$set(item, 'src', e.target.result)
          }
          reader.readAsDataURL(fileList[i])
          this.files.push(item)
          this.$store.commit('set_img_upload_cache', this.files) // 存储文件缓存
          this.$store.commit('set_img_status', 'selected') // 更新文件上传状态
        }
      }
    },
    // 上传图片
    submit () {
      let formData = new FormData()	# ③
      this.imgStore.forEach((item, index) => {
        item.name = 'imgFiles[' + index + ']' # ④
        formData.append(item.name, item.file)
      })
      formData.forEach((v, k) => console.log(k, ' => ', v))
      // 新建请求
      const xhr = new XMLHttpRequest()	# ⑤
      xhr.open('POST', this.url, true)
      xhr.send(formData)
      xhr.onload = () => {
        if (xhr.status === 200 || xhr.status === 304) {
          let datas = JSON.parse(xhr.responseText)
          console.log('response: ', datas)
          // 存储返回的地址
          let imgUrlPaths = new Set()	# ⑥
          datas.forEach(e => { // error === 0为成功状态
            e.error === 0 && imgUrlPaths.add(e.url)
          })
          this.$store.commit('set_img_paths', imgUrlPaths) // 存储返回的地址
          this.files = [] // 清空文件缓存
          this.index = 0 // 初始化序列号
          this.$store.commit('set_img_status', 'finished') // 更新文件上传状态
        } else {
          alert(`${xhr.status} 请求错误!`)
        }
      }
    },
    // 移除图片
    remove (index) {
      this.files.splice(index, 1)
      this.$store.commit('set_img_upload_cache', this.files) // 更新存储文件缓存
    },
    // 清空图片
    empty () {
      this.files.splice(0, this.files.length)
      this.$store.commit('set_img_upload_cache', this.files) // 更新存储文件缓存
      this.$store.commit('set_img_paths', [])
    }
  },
  beforeCreate () {
    this.$store.commit('set_img_status', 'ready') // 更新文件上传状态
  },
  destroyed () {
    this.$store.commit('set_img_upload_cache', [])
    this.$store.commit('set_img_paths', [])
  },
  watch: {
    imgStatus () {
      if (this.imgStatus === 'uploading') {
        this.submit()	# ⑦
      }
    },
    imgStore () {
      if (this.imgStore.length <= 0) {
        this.$store.commit('set_img_status', 'ready') // 更新文件上传状态
      }
    }
  }
}
</script>

<style lang="less" scoped>
...
</style>
```

以上代码中有一些注释序号，是此插件设计的主要思路，其他代码都比较容易理解，分别说下

- ① 选择文件后执行，`img_status`状态变为`selected`。
- ② 将带上传的图片文件转化为Base64格式，用于缩略图显示。
- ③ 创建一个表单对象，用于存储待上传的文件。
- ④ 注意这里的`name`属性值，暂时写死，后面设计打算从组件中指定`name`属性，如果是多文件的话，`name`属性的数组序号从0开始递增。
- ⑤ 未依赖任何Ajax请求插件，使用原生的`XMLHttpRequest`对象创建请求。
- ⑥ 存储上传成功后服务器返回的上传路径。
- ⑦ 检测上传状态，当在使用此插件时将`img_status`的状态设置为`uploading`时执行上传操作。 



# 使用

参考本项目的[GItHub](https://github.com/quanzaiyu/vue-easy-uploader)和[NPM](https://www.npmjs.com/package/vue-easy-uploader)。



# 注意

使用此插件时，需要与后端约定返回的数据格式，如下:

```
[{"error":0,"url":"\/uploads\/api\/201711\/25\/fde412bd83d3ec5d6a49769bd0c143cd.jpg"},{"error":0,"url":"\/uploads\/api\/201711\/25\/c6fd51f0388c63a0b6d350331c945fb1.jpg"}]
```

预览如下:

![](https://raw.githubusercontent.com/quanzaiyu/vue-easy-uploader/master/static/04.png)

返回的是一个上传后的路径数组，包括`error`和`url`字段，每个文件有自己的上传状态，当`error`为0的时候为上传成功，并返回上传后的路径`url`



# 改进

后续版本打算进行如下改进

1. 把表单的`name`属性名称通过组件传递。
2. 自定义上传成功后服务器响应的数据格式，比如自定义`error`的名称和其值所表示的状态。
3. 支持其他类型文件的上传，可以在组件中自行制定上传的文件类型，及其预览方式。

