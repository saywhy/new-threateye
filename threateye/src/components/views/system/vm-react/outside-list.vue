<template>
  <div id="outside_list"
       v-loading.fullscreen.lock="loading">
    <div class="outside_content">
      <div class="content_left content_common">
        <p class="title">恶意IP列表：</p>
        <p>
          <span>{{hostip}}/MaliciousIP?uname=账号&passwd=密码</span>
        </p>
        <div class="list_box">
          <p class="list_title_box">
            <span>IP:</span>
            <el-button class="btn_i"
                       @click="add_ip">添加</el-button>
          </p>
          <div class="item_box"
               v-for="(item,index) in outside_list.ip"
               @mouseenter="enter(index)"
               :class="item.class"
               @mouseleave="leave(index)">
            <span>{{item.addr}}</span>
            <img class="del_img"
                 v-if="item.icon"
                 @click="del_list(item)"
                 src="@/assets/images/common/del.png"
                 alt="">
          </div>
        </div>
      </div>
      <div class="content_left content_common">
        <p class="title">恶意域名列表：</p>
        <p>
          <span>{{hostip}}/MaliciousURL?uname=账号&passwd=密码</span>
        </p>
        <div class="list_box">
          <p class="list_title_box">
            <span>域名:</span>
            <el-button class="btn_i"
                       @click="add_url">添加</el-button>
          </p>
          <div class="item_box"
               v-for="(item,index) in outside_list.url"
               @mouseenter="enter_url(index)"
               :class="item.class==''?'':item.class"
               @mouseleave="leave_url(index)">
            <span>{{item.addr}}</span>
            <img class="del_img"
                 v-if="item.icon"
                 @click="del_list(item)"
                 src="@/assets/images/common/del.png"
                 alt="">
          </div>
        </div>
      </div>
      <div class="content_left content_common">
        <p class="title">恶意哈希列表：</p>
        <p>
          <span>{{hostip}}/MaliciousHash?uname=账号&passwd=密码</span>
        </p>
        <div class="list_box">
          <p class="list_title_box">
            <span>SHA256:</span>
            <el-button class="btn_i"
                       @click="add_hash">添加</el-button>
            <el-button class="btn_i"
                       @click="import_all">全部导入</el-button>
          </p>
          <div class="item_box"
               v-for="(item,index) in outside_list.hash"
               @mouseenter="enter_hash(index)"
               :class="item.class==''?'':item.class"
               @mouseleave="leave_hash(index)">
            <span>{{item.addr}}</span>
            <img class="del_img"
                 v-if="item.icon"
                 @click="del_list(item)"
                 src="@/assets/images/common/del.png"
                 alt="">
          </div>
        </div>
      </div>
    </div>
    <!-- 添加IP -->
    <el-dialog class="add_box pop_box"
               :close-on-click-modal="false"
               :modal-append-to-body="false"
               :visible.sync="outside_pop.ip.show">
      <img src="@/assets/images/emerge/closed.png"
           @click="closed_ip_box"
           class="closed_img"
           alt="">
      <div class="title">
        <div class="mask"></div>
        <span class="title_name">添加IP</span>
      </div>
      <div class="content">
        <div class="content_item">
          <p>
            <span class="title">IP</span>
            <span class="red_necessary">*</span>
          </p>
          <el-input class="select_box"
                    placeholder="请输入IP"
                    v-model="outside_pop.ip.ip"
                    clearable>
          </el-input>
        </div>
      </div>
      <div class="btn_box">
        <el-button @click="closed_ip_box"
                   class="cancel_btn">取消</el-button>
        <el-button class="ok_btn"
                   @click="add_list('1')">确定</el-button>
      </div>
    </el-dialog>
    <!-- 添加url -->
    <el-dialog class="add_box pop_box"
               :close-on-click-modal="false"
               :modal-append-to-body="false"
               :visible.sync="outside_pop.url.show">
      <img src="@/assets/images/emerge/closed.png"
           @click="closed_url_box"
           class="closed_img"
           alt="">
      <div class="title">
        <div class="mask"></div>
        <span class="title_name">添加域名</span>
      </div>
      <div class="content">
        <div class="content_item">
          <p>
            <span class="title">域名</span>
            <span class="red_necessary">*</span>
          </p>
          <el-input class="select_box"
                    placeholder="请输入域名"
                    v-model="outside_pop.url.url"
                    clearable>
          </el-input>
        </div>
      </div>
      <div class="btn_box">
        <el-button @click="closed_url_box"
                   class="cancel_btn">取消</el-button>
        <el-button class="ok_btn"
                   @click="add_list('2')">确定</el-button>
      </div>
    </el-dialog>
    <!-- 添加hash -->
    <el-dialog class="add_box pop_box"
               :close-on-click-modal="false"
               :modal-append-to-body="false"
               :visible.sync="outside_pop.hash.show">
      <img src="@/assets/images/emerge/closed.png"
           @click="closed_hash_box"
           class="closed_img"
           alt="">
      <div class="title">
        <div class="mask"></div>
        <span class="title_name">添加哈希</span>
      </div>
      <div class="content">
        <div class="content_item">
          <p>
            <span class="title">哈希</span>
            <span class="red_necessary">*</span>
          </p>
          <el-input class="select_box"
                    placeholder="请输入哈希"
                    v-model="outside_pop.hash.hash"
                    clearable>
          </el-input>
        </div>
      </div>
      <div class="btn_box">
        <el-button @click="closed_hash_box"
                   class="cancel_btn">取消</el-button>
        <el-button class="ok_btn"
                   @click="add_list('3')">确定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script type="text/ecmascript-6">
import { eventBus } from '@/components/common/eventBus.js';
export default {
  name: "outside_list",
  data () {
    return {
      loading: false,
      outside_list: {
        ip: [],
        url: [],
        hash: []
      },
      outside_pop: {
        ip: {
          show: false,
          ip: ""
        },
        url: {
          show: false,
          url: ""
        },
        hash: {
          show: false,
          hash: ""
        },
      },
      hostip: '',
    }
  },
  props: {
    option: {
      type: Object,
      default: () => { }
    }
  },
  mounted () {
    this.get_list('1')
    this.get_list('2')
    this.get_list('3')
    this.get_ip()
    this.check_passwd()
    // 必填；只能是1和2；动态类型，1Ip，2url
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
          if (status == '600') {
            this.$message(
              {
                message: msg,
                type: 'warning',
              }
            );
          }
        })
    },
    // 获取当前IP
    get_ip () {
      this.$axios.get('/yiiapi/linkage/get-hostip')
        .then(response => {
          console.log(response.data);
          this.hostip = response.data.data.url

        })
        .catch(error => {
          console.log(error);
        })

    },
    // 获取列表
    get_list (type) {
      this.$axios.get('/yiiapi/linkage/list', {
        params: {
          type: type,
          page: 1,
          rows: 9999
        }
      })
        .then(response => {
          console.log(response);
          if (type == '1') {
            this.outside_list.ip = response.data.data.data
            this.outside_list.ip.forEach(item => {
              this.$set(item, 'icon', false)
              this.$set(item, 'class', '')
            });
          }
          if (type == '2') {
            this.outside_list.url = response.data.data.data
            this.outside_list.url.forEach(item => {
              this.$set(item, 'icon', false)
              this.$set(item, 'class', '')
            })
          }
          if (type == '3') {
            this.outside_list.hash = response.data.data.data
            this.outside_list.hash.forEach(item => {
              this.$set(item, 'icon', false)
              this.$set(item, 'class', '')
            })
          }
        })
        .catch(error => {
          console.log(error);
        })
    },
    // 添加ip
    add_ip () {
      this.outside_pop.ip.show = true;
      this.outside_pop.ip.ip = ''
    },
    add_list (num) {
      var addr_params = ''
      switch (num) {
        case '1':
          if (this.outside_pop.ip.ip == '') {
            this.$message(
              {
                message: '请输入ip地址',
                type: 'warning',
              }
            );
            return false
          }
          addr_params = this.outside_pop.ip.ip
          break;
        case '2':
          if (this.outside_pop.url.url == '') {
            this.$message(
              {
                message: '请输入域名',
                type: 'warning',
              }
            );
            return false
          }
          addr_params = this.outside_pop.url.url
          break;
        case '3':
          if (this.outside_pop.hash.hash == '') {
            this.$message(
              {
                message: '请输入哈希',
                type: 'warning',
              }
            );
            return false
          }
          addr_params = this.outside_pop.hash.hash
          break;
        default:
          break;
      }
      this.$axios.post('/yiiapi/linkage/add', {
        addr: addr_params,
        type: num,
      })
        .then(response => {
          console.log(response);
          if (response.data.status == 1) {
            this.$message(
              {
                message: response.data.msg,
                type: 'error',
              }
            );
          } else if (response.data.status == 0) {
            switch (num) {
              case '1':
                this.get_list('1')
                this.outside_pop.ip.show = false;
                this.$message(
                  {
                    message: '添加IP成功',
                    type: 'success',
                  }
                );
                break;
              case '2':
                this.get_list('2')
                this.outside_pop.url.show = false;
                this.$message(
                  {
                    message: '添加域名成功',
                    type: 'success',
                  }
                );
                break;
              case '3':
                this.get_list('3')
                this.outside_pop.hash.show = false;
                this.$message(
                  {
                    message: '添加哈希成功',
                    type: 'success',
                  }
                );
                break;
              default:
                break;
            }
          }
        })
        .catch(error => {
          console.log(error);
        })
    },
    add_url () {
      this.outside_pop.url.show = true;
      this.outside_pop.url.url = ''
    },
    add_hash () {
      this.outside_pop.hash.show = true;
      this.outside_pop.hash.hash = ''
    },
    // 删除ip
    del_list (item) {
      console.log(item);
      this.$confirm('此操作删除信息, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        var id_list = []
        id_list.push(item.id)
        this.$axios.delete('/yiiapi/linkage/del', {
          data: {
            id: id_list
          }
        })
          .then(response => {
            console.log(response);
            if (response.data.status == 0) {
              this.get_list('1');
              this.get_list('2');
              this.get_list('3');
              this.$message(
                {
                  message: '删除成功！',
                  type: 'success',
                }
              );
            } else {
              this.$message(
                {
                  message: '删除失败！',
                  type: 'error',
                }
              );
            }
          })
          .catch(error => {
            console.log(error);
          })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        });
      });
    },
    closed_ip_box () {
      this.outside_pop.ip.show = false;
    },
    closed_url_box () {
      this.outside_pop.url.show = false;
    },
    closed_hash_box () {
      this.outside_pop.hash.show = false;
    },
    enter (num) {
      this.outside_list.ip.forEach((item, index) => {
        if (num == index) {
          item.icon = true
          item.class = 'active'
        }
      });
    },
    leave (index) {
      this.outside_list.ip[index].icon = false
      this.outside_list.ip[index].class = ''
    },
    enter_url (index) {
      this.outside_list.url[index].icon = true
      this.outside_list.url[index].class = 'active'
    },
    leave_url (index) {
      this.outside_list.url[index].icon = false
      this.outside_list.url[index].class = ''
    },
    enter_hash (index) {
      this.outside_list.hash[index].icon = true
      this.outside_list.hash[index].class = 'active'
    },
    leave_hash (index) {
      this.outside_list.hash[index].icon = false
      this.outside_list.hash[index].class = ''
    },
    import_all () {
      this.$confirm('确认导入平台发现的所有恶意程序哈希值？', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.loading = true;
        // yiiapi/linkage/import-all
        this.$axios.post('/yiiapi/linkage/import-all')
          .then(response => {
            this.loading = false;
            let {
              status,
              msg,
              data
            } = response.data;
            if (status == 0) {
              this.$message(
                {
                  message: '导入成功！',
                  type: 'success',
                }
              );
            }
          })
          .catch(error => {
            console.log(error);
          })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消导入'
        });
      })
    }
  }
};
</script>

<style scoped lang="less">
#outside_list {
  text-align: left;
  .outside_content {
    margin-top: 12px;
    display: flex;
    .content_common {
      flex: 1;
      word-break: break-all;
      white-space: pre-wrap;
      word-wrap: break-word;
      .title {
        padding-bottom: 8px;
        font-family: PingFangMedium;
        font-size: 16px;
        color: #333333;
        border-bottom: 1px solid #ececec;
      }
      .list_box {
        padding: 18px 0 14px 0;
        background: #f8f8f8;
      }
      .list_title_box {
        height: 34px;
        line-height: 34px;
        margin-bottom: 16px;
        padding: 0 26px 0 14px;
        .btn_i {
          width: 124px;
          height: 34px;
          padding: 0;
          background: #0070ff;
          color: #fff;
          float: right;
        }
      }
      .item_box {
        // height: 36px;
        line-height: 36px;
        padding: 0 26px 0 14px;
        position: relative;
        .del_img {
          cursor: pointer;
          width: 14px;
          height: 14px;
          position: absolute;
          top: 50%;
          right: 26px;
          transform: translateY(-50%);
        }
      }
      .active {
        background: #eef6ff;
        cursor: pointer;
      }
    }
    .content_left {
      margin-right: 22px;
    }
  }
}
</style>
<style lang='less'>
@import '../../../../assets/css/less/reset_css/reset_table.less';
@import '../../../../assets/css/less/reset_css/reset_pop.less';
#outside_list {
  .add_box {
    .el-dialog {
      width: 440px;
      .content_item {
        margin-top: 24px;
      }
      .el-input__inner {
        background: #f8f8f8;
        border: 0;
      }
      .btn_box {
        margin-top: 24px;
      }
    }
  }
}
</style>
