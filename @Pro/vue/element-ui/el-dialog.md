el-dialog
[toc]

# 坑
## 首次渲染问题
弹框未首次打开之前，是会不渲染弹框body中的内容的 固获取不到里边要操作的dom =null
- 利用extends我们可以保留组件来源的全部功能，只需加上我们需要的逻辑就行了
  <script>
    import { Dialog } from "element-ui";
    export default {
    extends: Dialog,
    props: {
        preload: {
        type: Boolean,
        default: false,
        },
    },
    created() {
        this.preload && (this.rendered = true);
    },
    };
  </script>
- 获取更新的dom,进行dom的相关操作
  利用this.$nextTick(()=>{
    doSomething();
  })
  等更新完成后,在操作 
