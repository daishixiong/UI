
## vue-custom-upload

### 这是一个 基于Vue项目的 自定义上传UI组件, 分为2种形态: 
- 1、avatar(头像模式-单个)
- 2、uploadList(照片墙模式 -- 照片墙)
 
#### use
- 1、下载安装依赖 
    - npm i vue-custom-upload;     
- 2、引入、注册     
    - import Upload form 'vue-custom-upload';
    - vue.use(Upload);
- 3、使用

#### Attribute

| 参数 | 说明 | 类型 | 可选值	| 默认值 |
| ------ | ------ | ------ | ------ | ----- |
| type        | 展示形态  | String  | avatar/uploadList | avatar |
| propKey     | 自定义 展示项标识（只有在 type='uploadList'时生效）| String  | - | uuid |
| action      | 服务器接收地址 | String | - | - |
| disabled    | 是否禁用 | Boolean | true/false | false |
| on-progress | 上传中回调 | function(file)  | - | - |
| on-success  | 上传成功回调 | function(file) | - | - |

#### way
| 方法名 | 说明 | 参数 |
| ------ | ------ | ------ |
| remove | 删除某一项, this.$refs.xxx.remove({uuid: 'xxx'})  | (file), 当前 项对象， 会根据对象的 标识字段(默认uuid)作为删除标识 |


##### 代码示例
```html
    // avatar形态
    <Upload
      ref="upLoad"
      :disabled="false"
      action="接收服务器地址"
      @on-progress="onProgress"
      @on-success="onSuccess">
        <div slot="uploadUI">
            。。。 此处写点击的 上传项 样式
        </div>
    </Upload>
    
    // 照片墙 形态
    <Upload
      ref="upLoad"
      type="uploadList" 
      propKey="uuid"     
      :disabled="false"
      action="接收服务器地址"
      @on-progress="onProgress"
      @on-success="onSuccess">
        <div slot="showUI" slot-scope="{file}">
            {{ file.value }}
            。。。 此处写 上传后 展示项 的样式
        </div>
        <div slot="uploadUI">
            。。。 此处写点击的 上传项 样式
        </div>
    </Upload>
```



