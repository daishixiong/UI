
## vue-custom-upload

### 这是一个 基于Vue项目的 自定义上传UI组件, 分为2种形态: 
- 1、avatar(头像模式-单个)
- 2、uploadList(照片墙模式 -- 照片墙)
 
#### use
- 1、下载安装依赖 
    - npm i vue-custom-upload;     
- 2、引入、注册     
    - import xxx form 'vue-custom-upload';
    - vue.use(xxx);
- 3、使用

##### 代码示例
```html
    // avatar形态
    <Upload
      ref="upLoad"
      :disabled="false"
      action="http://172.16.72.130:3000/upLoad"
      @on-progress="onProgress"
      @on-success="onSuccess">
        <div slot="uploadUI">
            。。。 此处写点击的 上传项 样式
        </div>
    </Upload>
    
    // 照片墙 形态
    <Upload
      ref="upLoad"
      type="uploadList"      // type 控制 形态 默认 avatar
      :disabled="false"
      action="http://172.16.72.130:3000/upLoad"
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

