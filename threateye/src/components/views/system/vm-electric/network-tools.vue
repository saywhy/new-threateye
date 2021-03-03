<template>
  <div id="network-tools"
       v-loading.fullscreen.lock="loading">
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
                 @click="Ping_search">Ping</el-button>
      <el-button class="btn_i"
                 @click="Telnet_search">Telnet</el-button>
      <div class="box">
        <div class="content">
          <p v-for="item in ping.data"
             class="item_style">
            {{item}}
          </p>
        </div>
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
      loading: false,
      ping: {
        ip: '',
        port: '',
        data: []
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
    // this.check_passwd()
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
          if (status == '602') {
            this.$message(
              {
                message: msg,
                type: 'warning',
              }
            );
            eventBus.$emit('reset')
          }
        })
    },
    Ping_search () {
      // 
      if (this.ping.ip == '') {
        this.$message(
          {
            message: 'ip地址不能为空',
            type: 'warning',
          }
        );
        return false;
      }
      this.loading = true;
      this.$axios.get('/yiiapi/seting/ping', {
        params: {
          ip: this.ping.ip
        }
      })
        .then(resp => {
          this.loading = false;
          let {
            status,
            msg,
            data
          } = resp.data;
          if (status == '602') {
            this.$message(
              {
                message: msg,
                type: 'warning',
              }
            );
            eventBus.$emit('reset')
          }
          if (status == 1) {
            this.$message(
              {
                message: msg,
                type: 'warning',
              }
            );
          } else if (status == 0) {
            this.ping.data = data
          }
        })
        .catch(error => {
          console.log(error);
        })
      // 在第一个写着Ping的按钮上面写“Ping”， ping的时候IP地址必选
      // 在第二个写着Ping的按钮上面写“Telnet”，在点击这个按钮的时候IP地址和端口号都必填
    },
    Telnet_search () {
      if (this.ping.ip == '') {
        this.$message(
          {
            message: 'ip地址不能为空',
            type: 'warning',
          }
        );
        return false;
      }
      if (this.ping.port == '') {
        this.$message(
          {
            message: '端口号不能为空',
            type: 'warning',
          }
        );
        return false;
      }
      this.loading = true;
      this.$axios.get('/yiiapi/seting/telnet', {
        params: {
          ip: this.ping.ip,
          port: this.ping.port
        }
      })
        .then(resp => {
          this.loading = false;
          let {
            status,
            msg,
            data
          } = resp.data;
          if (status == 1) {
            this.$message(
              {
                message: msg,
                type: 'warning',
              }
            );
          } else if (status == 0) {
            this.ping.data = data
          }
        })
        .catch(error => {
          console.log(error);
        })
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
  .box {
    width: 100%;
    padding-right: 24px;
    // border: 1px solid red;
    min-height: 400px;
  }
  .content {
    margin-top: 42px;
    margin-right: 42px;
    margin-bottom: 42px;
    padding: 24px;
    // border: 1px solid red;
    width: 100%;
    background: #f8f8f8;
    min-height: 400px;
    .item_style {
      color: #03a89e;
    }
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
