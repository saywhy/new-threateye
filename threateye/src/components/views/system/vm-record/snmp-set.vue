<template>
  <div id="snmp"
       v-loading.fullscreen.lock="loading">
    <div class="item_box">
      <div class="left">
        <span>SNMP开关</span>
      </div>
      <div class="right">
        <el-switch v-model="snmp.switch"
                   @change="set_switch">
        </el-switch>
      </div>
    </div>
    <div class="snmp_top"
         v-if="snmp.switch">
      <div class="item_box">
        <div class="left">
          <span>SNMP端口</span>
          <span class="red">*</span>
        </div>
        <div class="right">
          <el-input class="input_box"
                    placeholder="请输入SNMP端口"
                    v-model="snmp.port"
                    clearable>
          </el-input>
        </div>
      </div>
      <div class="item_box">
        <div class="left">
          <span>SNMP服务协议类型</span>
        </div>
        <div class="right">
          <el-input class="input_box"
                    placeholder="请输入SNMP服务协议类型"
                    v-model="snmp.type"
                    clearable>
          </el-input>
        </div>
      </div>
      <div class="item_box">
        <div class="left">
          <span>SNMP版本</span>
        </div>
        <div class="right">
          <el-select class="input_box"
                     v-model="snmp.edition"
                     @change='change_method'
                     placeholder="请选择SNMP版本">
            <el-option v-for="item in edition"
                       :key="item.value"
                       :label="item.name"
                       :value="item.value">
            </el-option>
          </el-select>
        </div>
      </div>
      <!-- v3 -->
      <div class="v3"
           v-if="snmp.v3_show">
        <!-- 用户名 -->
        <div class="item_box"
             v-if="snmp.v3_show">
          <div class="left">
            <span>用户名</span>
            <span class="red">*</span>
          </div>
          <div class="right">
            <el-input class="input_box"
                      placeholder="范围6-32位以内的字母、数字、下划线"
                      v-model="snmp.user"
                      clearable>
            </el-input>
          </div>
        </div>
        <!-- 认证加密方式 -->
        <div class="item_box"
             v-if="snmp.v3_show">
          <div class="left">
            <span>认证加密方式</span>
          </div>
          <div class="right">
            <el-select class="input_box"
                       v-model="snmp.encryption"
                       @change='change_encryption'
                       placeholder="请选择认证加密方式">
              <el-option v-for="item in encryption"
                         :key="item.value"
                         :label="item.name"
                         :value="item.value">
              </el-option>
            </el-select>
          </div>
        </div>
        <!-- 认证方式 -->
        <div class="item_box"
             v-if="!snmp.encryption_v3">
          <div class="left">
            <span>认证方式</span>
          </div>
          <div class="right">
            <el-select class="input_box"
                       v-model="snmp.authentication_type"
                       @change='change_authentication_type'
                       placeholder="请选择认证方式">
              <el-option v-for="item in authentication_type"
                         :key="item.value"
                         :label="item.name"
                         :value="item.value">
              </el-option>
            </el-select>
          </div>
        </div>
        <!-- 认证密码 -->
        <div class="item_box"
             v-if="!snmp.encryption_v3">
          <div class="left">
            <span>认证密码</span>
            <span class="red">*</span>
          </div>
          <div class="right">
            <el-input class="input_box"
                      placeholder="范围6-32位以内的字母、数字、下划线"
                      v-model="snmp.authentication_password"
                      show-password
                      clearable>
            </el-input>
          </div>
        </div>

        <!-- 加密方式 -->
        <div class="item_box"
             v-if="snmp.encryption_v1">
          <div class="left">
            <span>加密方式</span>
          </div>
          <div class="right">
            <el-select class="input_box"
                       v-model="snmp.password_type"
                       @change='change_password_type'
                       placeholder="请选择认证方式">
              <el-option v-for="item in password_type"
                         :key="item.value"
                         :label="item.name"
                         :value="item.value">
              </el-option>
            </el-select>
          </div>
        </div>
        <!-- 加密密码 -->
        <div class="item_box"
             v-if="snmp.encryption_v1">
          <div class="left">
            <span>加密密码</span>
            <span class="red">*</span>
          </div>
          <div class="right">
            <el-input class="input_box"
                      placeholder="范围6-32位以内的字母、数字、下划线"
                      v-model="snmp.authentication_password"
                      show-password
                      clearable>
            </el-input>
          </div>
        </div>
      </div>
      <!-- v3 -->

      <div class="item_box"
           v-if="!snmp.v3_show">
        <div class="left">
          <span>团体字</span>
          <span class="red">*</span>
        </div>
        <div class="right">
          <el-input class="input_box"
                    placeholder="请输入团体字"
                    clearable>
          </el-input>
        </div>
      </div>
    </div>
    <div class="item_box">
      <div class="left">
      </div>
      <div class="right">
        <el-button class="btn_i w_180">MIB文件下载</el-button>
        <el-button class="btn_i w_100">保存</el-button>
      </div>
    </div>
    <div class="snmp_bom"
         v-if="snmp.v3_show">
      <div class="item_box">
        <div class="left">
          <span>SNMP Trap服务开关</span>
        </div>
        <div class="right">
          <el-switch v-model="snmp.Trap"
                     @change="set_switch">
          </el-switch>
        </div>
      </div>
      <div class="item_box"
           v-if="snmp.v3_show">
        <div class="left">
        </div>
        <div class="right">
          <el-button class="btn_i w_180">编辑SNMP Trap</el-button>
          <el-button class="btn_i w_100">保存</el-button>
        </div>
      </div>
    </div>

  </div>
</template>

<script type="text/ecmascript-6">
import moment from 'moment'
export default {
  name: "snmp_set",
  data () {
    return {
      loading: false,
      edition: [
        {
          name: 'v1',
          value: 1
        },
        {
          name: 'v2c',
          value: 2
        },
        {
          name: 'v3',
          value: 3
        },
      ],
      authentication_type: [{
        name: 'SHA',
        value: 1
      },
      {
        name: 'MD5',
        value: 2
      }],
      encryption: [
        {
          name: '认证及加密',
          value: 1
        },
        {
          name: '仅认证',
          value: 2
        },
        {
          name: '不认证不加密',
          value: 3
        },
      ],
      password_type: [
        {
          name: 'DES',
          value: 1
        },
        {
          name: 'AES',
          value: 2
        }
      ],
      snmp: {
        v1_show: false,
        v3_show: false,
        encryption_v1: true,
        encryption_v2: false,
        encryption_v3: false,
        switch: false,
        Trap: false,
        port: '161',
        type: 'UDP',
        edition: 1,
        encryption: 1,
        authentication_type: 1,
        password_type: 1,
        authentication_password: '',
        user: ''
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
    this.get_data()
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
    // 获取列表
    get_data () {
      this.loading = true
      this.$axios.get('/yiiapi/snmp/index')
        .then(response => {
          this.loading = false;
          console.log(response);
        })
        .catch(error => {
          console.log(error);
        })
    },
    set_switch () {
      console.log('111');
      this.snmp.v1_show = this.snmp.switch
    },
    change_method () {
      switch (this.snmp.edition) {
        case 1:
          this.snmp.v3_show = false;
          break;
        case 2:
          this.snmp.v3_show = false;
          break;
        case 3:
          this.snmp.v3_show = true;
          break;
        default:
          break;
      }

    },
    // 认证及加密方式
    change_encryption () {
      console.log(this.snmp.encryption);
      switch (this.snmp.encryption) {
        case 1:
          this.snmp.encryption_v1 = true
          this.snmp.encryption_v2 = false
          this.snmp.encryption_v3 = false
          break;
        case 2:
          this.snmp.encryption_v1 = false
          this.snmp.encryption_v2 = true
          this.snmp.encryption_v3 = false
          break;
        case 3:
          this.snmp.encryption_v1 = false;
          this.snmp.encryption_v2 = false
          this.snmp.encryption_v3 = true
          break;
        default:
          break;
      }

    },
    // 认证方式
    change_authentication_type () {

    },
    // 加密方式
    change_password_type () {

    }
  },
  filters: {
    formatDate: function (value) {
      return moment(value).format('YYYY-MM-DD HH:mm:ss')
    }
  },
};
</script>

<style scoped lang="less">
#snmp {
  text-align: left;
  /deep/.el-switch__core {
    width: 40px !important;
  }
  .red {
    color: #ff5f5c;
  }
  .item_box {
    // border: 1px solid red;
    height: 42px;
    line-height: 42px;
    display: flex;
    margin: 10px 0;
    .left {
      // border: 1px solid red;
      width: 180px;
      text-align: right;
      padding-right: 20px;
    }
    .right {
      flex: 1;
      width: 136px;
      .btn_i {
        height: 42px;
        padding: 0;
        background: #0070ff;
        color: #fff;
        margin: 0;
      }
      .w_180 {
        width: 200px;
        margin-right: 40px;
      }
      .w_100 {
        width: 120px;
      }
      .input_box {
        width: 360px;
      }
    }
  }
  .snmp_top {
  }
}
</style>
<style lang='less'>
@import '../../../../assets/css/less/reset_css/reset_table.less';
@import '../../../../assets/css/less/reset_css/reset_pop.less';
#fault_log {
}
</style>