<template>
  <div id="network-tools">
    <div>
      <span class="red">*</span>
      <el-input class="input_box"
                placeholder="请输入IP地址"
                v-model="ping.ip"
                clearable>
      </el-input>
      <el-input class="input_box_port"
                placeholder="请输入端口号"
                v-model="ping.port"
                clearable>
      </el-input>
      <el-button class="btn_i"
                 plain
                 disabled
                 @click="Ping_search">Ping</el-button>
      <el-button class="btn_i"
                 disabled
                 @click="Telnet_search">Telnet</el-button>
      <div class="content">

      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import { eventBus } from '@/components/common/eventBus.js';
export default {
  name: "network-tools",
  data () {
    return {
      ping: {
        ip: '',
        port: ''
      }

    };
  },
  props: {
    option: {
      type: Object,
      default: () => { }
    }
  },
  mounted () {
    this.check_passwd()
  },
  methods: {
    // 测试密码过期
    check_passwd () {
      this.$axios.get('/yiiapi/site/check-passwd-reset')
        .then((resp) => {
          let {
            status,
            msg,
            data
          } = resp.data;
          if (status != 0) {
            for (let key in msg) {
              if (key == 600) {
                this.$message(
                  {
                    message: msg[key],
                    type: 'warning',
                  }
                );
              }
              if (key == 602) {
                this.$message(
                  {
                    message: msg[key],
                    type: 'warning',
                  }
                );
                eventBus.$emit('reset');
              }
            }
          }
        })
    },
    Ping_search () {
      // 在第一个写着Ping的按钮上面写“Ping”， ping的时候IP地址必选
      // 在第二个写着Ping的按钮上面写“Telnet”，在点击这个按钮的时候IP地址和端口号都必填
    },
    Telnet_search () {

    }

  }
};
</script>

<style scoped lang="less">
#network-tools {
  text-align: left;
  .red {
    color: #ff5f5c;
  }
  .input_box {
    width: 200px;
    margin-right: 24px;
  }
  .input_box_port {
    width: 200px;
  }
  .btn_i {
    margin-left: 24px;
    padding: 0;
    width: 102px;
    height: 38px;
    background: #0070ff;
    color: #fff;
  }
  .content {
    margin-top: 42px;
    // border: 1px solid red;
    width: 800px;
    background: #f8f8f8;
    height: 400px;
  }
}
</style>
<style lang='less'>
#proxy-server {
  .el-input__inner {
    background: #f8f8f8;
    border: 0;
  }
  .el-switch__core {
    width: 40px !important;
  }
}
</style>
