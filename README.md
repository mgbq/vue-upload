# vue-upload
轻量级的vue上传组件，支持查看大图、播放音频和视频  
## install
```sh
git clone https://github.com/laivv/vue-upload.git
```
## usage
```js
import upload from './upload'
```
### 使用组件
```html
<upload v-model="fileList"></upload>

<script>
  export default {
    data(){
      return {
        fileList:[]
      }
    }
  }
</script>  
```


### 给组件绑定初始列表
```html
<upload v-model="fileList"></upload>

<script>
 export default {
    data(){
      return {
        fileList:[
          {
            src:'/a1.jpg',
            type:'image' //如果不设置type属性，默认为 `file` ，将不能作为图片预览
          },
          {
            src:'/a2.avi',
            type:'video'
          },
          {
            src:'/a3.mp3',
            type:'audio'
          },
          {
            src:'/a4.txt',
            type:'text'
          },
          {
            src:'/a5.zip',
            type:'rar'
          },
          {
            src:'/a5.7z',
            type:'rar'
          },
          {
            src:'/a6-unknow-file-type',
            type:'file'
          },
        ]
      }
    }
  }

</script>  
```



### 获取上传状态

```html
<upload ref="upload" v-model="fileList"></upload>
<button @click="submit">提交</button>

<script>
 export default {
    data(){
      return {
        fileList:[]
      }
    },
    methods:{
      submit(){
         const isCompleted = this.$refs.upload.isCompleted() //true上传完毕 ;false未上传完毕
         if(!isCompleted){
            alert("还有文件正在上传，不能提交")
          }
      }
  
    }
  }

</script>  
```

### 获取上传成功的文件列表

```html
<upload ref="upload" v-model="fileList"></upload>
<button @click="submit">提交</button>

<script>
  export default {
    data(){
      return {
        fileList:[]
      }
    },
    methods:{
     submit(){
        //获取上传是否完毕，无论上传成功还是失败
         const isCompleted = this.$refs.upload.getUploadStatus() //true上传完毕 ;false未上传完毕
         if(!isCompleted){
            alert("还有文件正在上传，不能提交")
          }else{
              //获取上传成功的文件列表
              const successFiles = this.$refs.upload.getSuccessFiles()
              //提交数据 do ajax
          }
      }
    }
  }

</script>  
```


### 获取 所有/失败 的文件列表

```html
<upload ref="upload" v-model="fileList"></upload>
<button @click="getErrorFiles">获取上传失败的文件列表</button>
<button @click="getFiles">获取所有文件列表</button>

<script>
  export default {
    data(){
      return {
        fileList:[]
      }
    },
    methods:{
     getErrorFiles(){
        //上传失败的文件列表
        const errorFiles = this.$refs.upload.getErrorFiles()
        console.log(errorFileList)
      },
      getFiles() {
        //所有文件列表，同 `v-model` 绑定的list一样
        const files = this.$refs.upload.getFiles()
        console.log(files)
      },
    }
  }

</script>  
```



### 组件只读展示
将组件的`readonly`属性设置为`true`则仅将组件作展示用途，不能上传或编辑
```html
<upload v-model="fileList" :readonly="true"></upload>

<script>
 export default {
    data(){
      return {
        fileList:[
          {
            src:'/a1.jpg',
            type:'image' 
          },
          {
            src:'/a2.avi',
            type:'video'
          },
          {
            src:'/a3.mp3',
            type:'audio'
          },
          {
            src:'/a4.txt',
            type:'text'
          },
          {
            src:'/a5.zip',
            type:'rar'
          },
          {
            src:'/a5.7z',
            type:'rar'
          },
          {
            src:'/a6-unknow-file-type',
            type:'file'
          },
        ]
      }
    }
  }

</script>  
```




### 单独使用组件的预览窗功能
当不想使用组件自带的文件列表，但又想使用组件的预览功能时，可以仅使用组件的预览功能  
将组件的`readonly`属性设置为`true`，并且将`showFileList`设置为`false`能够起到不显示组件的作用
```html
<upload ref="upload" :readonly="true" :show-file-list="false" v-model="currentFileList"></upload>
<div>
  <img :src="image.src" v-for="(image,index) in image1" @click="showPreview(index,image1)">
</div>
<div>
  <img :src="image.src" v-for="(image,index) in image2" @click="showPreview(index,image2)">
</div>

<script>
 export default {
    data(){
      return {
        image1:[
          {
            src:'/a1.jpg',
            type:'image'
          },
          {
            src:'/a2.jpg',
            type:'image'
          }
        ],
         image2:[
          {
            src:'/b1.jpg',
            type:'image'
          },
          {
            src:'/b2.jpg',
            type:'image'
          }
        ],
        currentFileList:[]
      }
    },
    methods:{
      showPreview(index,files){
          this.currentFileList = files
          this.$refs.upload.openPreviewer(index)
      }
    }
  }

</script>  
```



### 属性
|属性名|说明|类型|默认值|
|-------|----------------|-----|------|
|url|上传的url地址|String|http://up.qiniu.com|
|v-model|绑定的数据|Array|[]|
|name|上传的参数名|String|`file`|
|auto-upload|是否在选择文件后立即上传|Boolean|`true`|
|multiple|是否支持同时选择多个文件|Boolean|true|
|max-file-size|单个文件大小限制（MB）|Number|无|
|max-file-count|最多上传多少个文件|Number|无|
|accept-list|可上传的文件扩展名列表(如 ['jpg','png'])|Array|无|
|thumb-query|如果上传的是图片，此参数为缩略图query字符串,(如'?image/height/200/width/100')|String|无|
|token-url|获取上传token的url列表(如 ['/api/token1','/api/token2']),接口应当返回一个{key:value}形式的数据|Array|无|
|readonly|是否只读模式,设为true将不能上传，只作展示使用|Boolean|false|
|enable-upload|是否开启上传，某些条件下不允许用户再进行上传操作可设置为false|Boolean|true|
|show-file-name|显示上传的文件名|Boolean|false|
|show-file-list|显示上传的文件列表|Boolean|true|
|show-preview-file-name|显示全屏预览器中的文件名|Boolean|false|
|list-type|文件列表的显示方式,可选值 'list' 或 'card'|String|card|
|is-qiniu|是否上传到七牛云，设置true将使用七牛所需参数来上传文件|Boolean|true|


### 回调钩子
|属性名|说明|回调参数|回调参数说明|
|-------|--------------------|-----|-------------|
|before-file-add|当某个文件在添加到上传列表之前调用，通过返回true或false来决定该文件是否被添加|Function(file)|即将被添加的文件|
|on-file-success|当某个文件上传成功时调用|Function(file,response,param?)|file:上传成功的那个文件,response:上传成功服务器返回的数据,param:如果有获取token相关数据，param则是包含了token相关的数据|
|on-file-error|当某个文件上传失败时调用|Function(file)|某个上传失败的文件|
|on-upload-complete|当所有文件上传完毕时调用（无论上传成功还是失败）|Function()|无|
|on-file-click|当点击文件列表中的某个文件时调用，通过返回true或false来决定是否打开预览窗|Function(file)|被点击的那个文件|
|on-file-remove|当移除文件列表中的某个文件时调用，通过返回true、false或Promise来决定该文件是否移除|Function(file)|将被移除的那个文件|
|on-preview-close|当预览窗关闭的时候调用|Function()|无|
|on-preview-switch|当在预览窗中切换上一个或下一个文件时调用|Function(index,file)|index:当前文件在文件列表filelist中的索引;file:当前文件|
|on-file-type-error|当上传的文件类型不符合设定的值时调用|Function(files)|不符合条件的文件列表|
|on-file-count-error|当上传的文件超过限定的个数时调用|Function(files)|超出部分的文件列表|
|on-file-size-error|当上传的单个文件大小超过设定的值时调用|Function(files)|超过设定大小的文件列表|
|token-func|自定义获取token的方法|Function(done:(data)=>void)|done(data:{[key:any]:any}) ;data为自定义方法返回给组件的数据|

### 组件方法  
使用`vm.$refs.uploadRef.methodName()`的形式来调用   
|方法名|参数|说明|
|-----|-----|------|
|getSuccessFiles|-|获取上传成功的文件列表|
|getErrorFiles|-|获取上传失败的文件列表|
|getUploadingFiles|-|获取正在上传的文件列表|
|getFiles|-|获取所有文件列表（含 上传成功/失败/上传中/等待上传 的文件）|
|isCompleted|-|获取所有文件是否上传完毕|
