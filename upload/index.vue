<script type="text/ecmascript-6">
/**
 * @Description: 上传组件
 * @author DaiSX
 * @date 2019-08-15
 */
import UpLoad from './upload.vue';
import UploadList from './upload-list.vue';

export default {
  name: 'Upload',
  components: {
    UpLoad,
    UploadList
  },

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
    data: { // ajax参数 可选
      type: Object,
      default () {
        return {};
      }
    }
  },

  data () {
    return {
      uploadFiles: [],
      i: 100
    };
  },

  watch: {
    uploadFiles: {
      deep: true,
      handler (list) {
        list.map(item => {
          item.uuid = item.uuid || new Date().getTime();
          return item;
        });
      }
    }
  },

  methods: {
    handleProgress (res, rawFile) {
      this.$emit('on-progress', res, rawFile);
    },
    handleSuccess (res, rawFile) {
      this.$nextTick(_ => {
        this.uploadFiles.push(res);
        this.$emit('on-success', res, rawFile);
      });
    },
    handleError (res, rawFile) {
      this.$emit('on-error', res, rawFile);
    },
    handleRemove (file) {
      let i = this.uploadFiles.findIndex(item => {
        return +item.value === +file.value;
      });
      if (i !== -1) {
        this.uploadFiles.splice(i, 1);
      }
      console.log('remove123', file);
    }
  },

  render () {
    const uploadData = {
      props: {
        type: this.type,
        action: this.action,
        multiple: this.multiple,
        name: this.name,
        data: this.data,
        accept: this.accept,
        disabled: this.disabled,
        'on-progress': this.handleProgress,
        'on-success': this.handleSuccess,
        'on-error': this.handleError
      },
      ref: 'upload'
    };

    const uploadListData = {
      props: {
        files: this.uploadFiles
      },
      on: {
        'remove': this.handleRemove
      },
      ref: 'uploadList'
    };

    const uploadUI = this.$slots.uploadUI || this.$slots.default;

    if (this.type === 'uploadList') {
      return (<div class='upload-list'>
            <upload-list { ...uploadListData }>
                {
                    (props) => {
                      if (this.$scopedSlots.showUI) {
                        return this.$scopedSlots.showUI({
                          file: props.file
                        });
                      }
                    }
                }
            </upload-list>
            <up-load { ...uploadData }>{ uploadUI }</up-load>
        </div>);
    }
    return (<up-load {...uploadData}>{ uploadUI }</up-load>);
  }
};
</script>
<style lang="less" scoped>
    .upload-list {
        display: flex;
        justify-content: flex-start;
        flex-wrap: wrap;
    }
</style>
