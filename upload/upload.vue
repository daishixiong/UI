<script type="text/ecmascript-6">
/**
* @Description: 上传组件
* @author DaiSX
* @date 2019-08-15
*/
import ajax from './ajax';

function noop () {}

export default {
  props: {
    type: {
      type: String, // avatar uploadList
      default: 'avatar'
    },
    disabled: {
      type: Boolean,
      default: false
    },
    name: {
      type: String,
      default: 'file'
    },
    multiple: {
      type: Boolean,
      default: true
    },
    accept: {
      type: String,
      default: 'image/*'
    },
    action: {
      type: String,
      default: 'http://172.16.73.36:3000/upLoad'
    },
    onSuccess: {
      type: Function,
      default: noop
    },
    onError: {
      type: Function,
      default: noop
    },
    onProgress: {
      type: Function,
      default: noop
    },
    httpRequest: {
      type: Function,
      default: ajax
    },
    data: { // ajax参数 可选
      type: Object,
      default () {
        return {};
      }
    }
  },

  methods: {
    isIosOrAndroid () {
      var u = navigator.userAgent;
          // ios终端
      if (u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/)) {
        return 'ios';
      }
          // android终端
      if (u.indexOf('Android') > -1 || u.indexOf('Adr') > -1) {
        return 'android';
      }
      return false;
    },
    handleClick (e) {
      if (!this.disabled) {
        if (this.isIosOrAndroid() === 'ios') { // ios 得触发2次方法
          this.$refs.input.click();
          this.$refs.input.click();
          console.log('1');
        } else {
          this.$refs.input.click();
          console.log('2');
        }
      }
    },
    handleChange (e) {
      const files = e.target.files;
      let postFiles = Array.prototype.slice.call(files);
      postFiles.forEach(rawFile => {
        this.upload(rawFile);
      });
    },
    upload (rawFile) {
      this.$refs.input.value = null;
      return this.post(rawFile);
    },
    post (rawFile) {
      const options = {
        file: rawFile,
        data: this.data,
        filename: this.name,
        action: this.action,
        onProgress: e => {
          this.onProgress(e, rawFile);
        },
        onSuccess: res => {
          this.onSuccess(res, rawFile);
        },
        onError: err => {
          console.log('报错了');
          this.onError(err, rawFile);
        }
      };
      const req = this.httpRequest(options);
      if (req && req.then) {
        req.then(options.onSuccess, options.onError);
      }
    }
  },

  render () {
    let {
        name,
        accept,
        multiple,
        handleChange,
        handleClick
    } = this;
    const data = {
      on: {
        click: handleClick
      }
    };
    const customUI = this.$slots.content || this.$slots.default;

    return (<div {...data} class='upload'>
          {customUI}
          <input class="upload_input" type="file" ref="input" name={name} on-change={handleChange} multiple={multiple} accept={accept}/>
      </div>);
  }
};
</script>
<style lang="less">
    .upload_input {
        display: none;
        opacity: 0;
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        width: 100%;
    }

</style>
