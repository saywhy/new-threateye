<template>
  <div class="home_overview"
       v-loading.fullscreen.lock="loading"
       v-cloak>
    <div class="container">

      <!-- 第一排 -->
      <el-row class="container_top  container_item"
              :gutter="10">
        <el-col :span="6">
          <div class="top_item"
               @click="sys_state">
            <div class="title">
              <p>
                <span class="title_left">系统状态监控</span>
              </p>
            </div>
            <div class="legend">
              <span class="legend_icon color8"></span>
              <span class="legend_title">预警:{{top_left.warning_count}}</span>
              <span class="legend_icon color3"></span>
              <span class="legend_title">健康:{{top_left.healthy_count}}</span>
              <span class="legend_icon color4"></span>
              <span class="legend_title">离线:{{top_left.offline_count}}</span>
            </div>
            <div class="top_left_content">
              <top-left :top_left="top_left"
                        v-if="top_left_show"></top-left>
            </div>
          </div>
        </el-col>
        <el-col :span="12">
          <div class="top_item">
            <div class="title">
              <p>
                <span class="title_left">流量文件监控</span>
                <span class="title_right">
                  <span class="title_right_icon color6"></span>
                  <span>流量(M/s)</span>
                  <span class="title_right_icon color2"></span>
                  <span>文件(个/s)</span>
                </span>
              </p>
            </div>
            <div class="top_mid_content">
              <div class="content_top">
                <top-mid-flow :top_mid="top_mid"
                              v-if="top_mid_show"></top-mid-flow>
              </div>
              <div class="content_bom">
                <top-mid-file :top_mid="top_mid"
                              v-if="top_mid_show"></top-mid-file>
              </div>
            </div>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="top_item">
            <div class="title">
              <p>
                <span class="title_left">协议统计</span>
              </p>
            </div>
            <div class="legend">
              <p class="legend_title">单位(P/s)</p>
            </div>
            <div class="top_right_content">
              <top-right :top_right="top_right"
                         v-if="top_right_show"></top-right>
            </div>
          </div>
        </el-col>
      </el-row>

      <!-- 第二排 -->
      <el-row class="container_bom  container_item"
              :gutter="10">
        <el-col :span="9">
          <div class="top_item">
            <div class="title">
              <p>
                <span class="title_left">告警</span>
                <span class="title_right">
                  <span class="title_right_icon color3"></span>
                  <span>低危</span>
                  <span class="title_right_icon color5"></span>
                  <span>中危</span>
                  <span class="title_right_icon color4"></span>
                  <span>高危</span>
                </span>
              </p>
            </div>
            <div class="bom_left_content">
              <div class="content_top">
                <mid-left :mid_left="mid_left"
                          v-if="mid_left_show"></mid-left>
              </div>
            </div>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="top_item">
            <div class="title">
              <p>
                <span class="title_left">未处理告警</span>
                <span class="title_right">
                  <span class="title_right_icon color3"></span>
                  <span>低危</span>
                  <span class="title_right_icon color5"></span>
                  <span>中危</span>
                  <span class="title_right_icon color4"></span>
                  <span>高危</span>
                </span>
              </p>
            </div>
            <div class="bom_mid_content">
              <mid-mid :mid_mid="mid_mid"
                       v-if="mid_mid_show"></mid-mid>
            </div>
          </div>
        </el-col>
        <el-col :span="9">
          <div class="top_item">
            <div class="title">
              <p>
                <span class="title_left">Top5威胁类型</span>
              </p>
            </div>
            <div class="bom_right_content">
              <mid-right :mid_right="mid_right"
                         v-if="mid_right_show"></mid-right>
            </div>
          </div>
        </el-col>
      </el-row>

      <!-- 第三排 -->
      <el-row class="container_bom  container_item"
              :gutter="10">
        <el-col :span="6">
          <div class="top_item">
            <div class="title">
              <p>
                <span class="title_left">Top5威胁</span>
              </p>
            </div>
            <div class="bom_left_content">
              <div class="content_top">
                <bom-left :bom_left="bom_left"
                          v-if="bom_left_show"></bom-left>
              </div>
            </div>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="top_item">
            <div class="title">
              <p>
                <span class="title_left">Top5风险资产</span>
              </p>
            </div>
            <div class="bom_mid_content">
              <bom-mid :bom_mid="bom_mid"
                       v-if="bom_mid_show"></bom-mid>
            </div>
          </div>
        </el-col>
        <el-col :span="12">
          <div class="top_item">
            <div class="title">
              <p>
                <span class="title_left">最新警告</span>
              </p>
            </div>
            <div class="bom_right_content">
              <bom-right :bom_right="bom_right"
                         v-if="bom_right_show"></bom-right>
            </div>
          </div>
        </el-col>
      </el-row>
    </div>
    <div class="">
      <!-- <sys-monitor></sys-monitor> -->
    </div>
    <el-dialog class="sys_box"
               width="840"
               :close-on-click-modal="false"
               :modal-append-to-body="false"
               :visible.sync="el_dialog">
      <img src="@/assets/images/emerge/closed.png"
           @click="closed_sys_box"
           class="closed_img"
           alt="">
      <div class="title">
        <div class="mask"></div>
        <span class="title_name">拓扑图</span>
      </div>
      <div id="graph">
      </div>
    </el-dialog>
    <el-dialog class="sys_detail"
               width="840"
               :close-on-click-modal="false"
               :modal-append-to-body="false"
               :visible.sync="equipment_detail.show">
      <img src="@/assets/images/emerge/closed.png"
           @click="closed_detail"
           class="closed_img"
           alt="">
      <div class="title">
        <div class="mask"></div>
        <span class="title_name">
          <span>{{equipment_detail.title.name}}</span>
          <span>{{equipment_detail.title.ip}}</span>
          <span>{{equipment_detail.title.type}}</span>
          <span>的健康情况</span>
        </span>
      </div>
      <div class="sys_detail_content">
        <div class="detail_item">
          <div id='cpu'></div>
        </div>
        <div class="detail_item">
          <div id='flow_echarts'></div>
        </div>
      </div>
    </el-dialog>
  </div>
</template>

<script type="text/ecmascript-6">
import topLeft from "./vm-home/top-left";
import topMidFlow from "./vm-home/top-mid-flow";
import topMidFile from "./vm-home/top-mid-file";
import topRight from "./vm-home/top-right";

import midLeft from "./vm-home/mid-left";
import midMid from "./vm-home/mid-mid";
import midRight from "./vm-home/mid-right";

import bomLeft from "./vm-home/bom-left";
import bomMid from "./vm-home/bom-mid";
import bomRight from "./vm-home/bom-right";

import { eventBus } from '@/components/common/eventBus.js';

import { isSynthetical } from "../../../assets/js/validate";
import ImagesIcon from "@/assets/js/icon";
export default {
  name: "system_control_move",
  data () {
    return {
      loading: false,
      top_left: {},
      top_left_show: false,

      top_mid: {},
      top_mid_show: false,

      top_right: {},
      top_right_show: false,
      ///////////

      mid_left: [],
      mid_left_show: false,

      mid_mid: [],
      mid_mid_show: false,

      mid_right: [],
      mid_right_show: false,

      ///////////
      bom_left: [],
      bom_left_show: false,

      bom_mid: [],
      bom_mid_show: false,

      bom_right: [],
      bom_right_show: false,
      //-----
      el_dialog: false,
      state_detail: false,
      equipment: {
        probe: [],
        engine: [],
        engine_probe: [],
        sandbox: [],
        management: [],
        management_probe: [],
        echart_array: [],
        links_array: [],
      },
      equipment_detail: {
        show: false,
        cpu: [],
        mem: [],
        disk: [],
        statistics_time: [],
        flow: [],
        title: {
          type: '',
          ip: '',
          name: '',
        }
      },
    };
  },
  created () {
    this.check_passwd();
    //第一排
    this.init_top_left();
    this.init_top_mid();
    this.init_top_right();
    //第二排
    this.init_mid_left();
    this.init_mid_mid();
    this.init_mid_right();
    //第三排
    this.init_bom_left();
    this.init_bom_mid();
    this.init_bom_right();
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
          if (status == 602) {
            this.$message(
              {
                message: msg,
                type: 'warning',
              }
            )
            eventBus.$emit('reset')
          }
        })
    },
    //第一排（左）
    init_top_left () {
      this.$axios.get('/yiiapi/alert/system-state')
        .then((resp) => {
          // console.log(resp)
          let {
            status,
            data
          } = resp.data;
          if (status == 0) {

            if (!isSynthetical(data)) {
              return false;
            } else {
              this.top_left = data;
              this.top_left_show = true;
            }

          }
        })
    },
    //第一排（中）
    init_top_mid () {
      this.$axios.get('/yiiapi/alert/flow-file-statistics')
        .then((resp) => {
          // console.log(resp)
          this.top_mid = {};
          let {
            status,
            data
          } = resp.data;

          if (status == 0) {
            if (!isSynthetical(data)) {
              return false;
            } else {
              this.top_mid = data;
              this.top_mid_show = true;
            }
          }
        })
    },
    //第一排（右）
    init_top_right () {
      this.$axios.get('/yiiapi/alert/protocol-flow-statistics')
        .then((resp) => {
          let {
            status,
            data
          } = resp.data;
          //console.log(data)
          if (status == 0) {

            if (!isSynthetical(data)) {
              return false;
            } else {
              this.top_right = data;
              this.top_right_show = true;
            }

          }
        })
    },

    //第二排（左）
    init_mid_left () {
      this.$axios.get('/yiiapi/alert/get-last7-days-alarm')
        .then((resp) => {
          let {
            status,
            data
          } = resp.data;
          if (status == 0) {

            if (!isSynthetical(data)) {
              return false;
            } else {
              this.mid_left = data;
              this.mid_left_show = true;
            }
          }
        })
    },
    //第二排（中）
    init_mid_mid () {
      this.$axios.get('/yiiapi/alert/untreated-alarm-type')
        .then((resp) => {
          let {
            status,
            data
          } = resp.data;
          if (status == 0) {
            if (!isSynthetical(data)) {
              return false;
            } else {
              this.mid_mid = data;
              this.mid_mid_show = true;
            }
          }
        })
    },
    //第二排（右）
    init_mid_right () {
      this.$axios.get('/yiiapi/alert/threat-type')
        .then((resp) => {

          let {
            status,
            data
          } = resp.data;
          if (status == 0) {

            if (!isSynthetical(data)) {
              return false;
            } else {
              this.mid_right = data;
              this.mid_right_show = true;
            }
          }
        })
    },


    //第三排（左）
    init_bom_left () {
      this.$axios.get('/yiiapi/alert/threat-top5')
        .then((resp) => {
          // /console.log(resp)
          let {
            status,
            data
          } = resp.data;
          if (status == 0) {
            if (!isSynthetical(data)) {
              return false;
            } else {
              this.bom_left = data;
              this.bom_left_show = true;
            }
          }
        })
    },
    //第三排（中）
    init_bom_mid () {
      this.$axios.get('/yiiapi/alert/risk-asset-top5')
        .then((resp) => {
          // console.log(resp)
          let {
            status,
            data
          } = resp.data;
          if (status == 0) {

            if (!isSynthetical(data)) {
              return false;
            } else {
              this.bom_mid = data;
              this.bom_mid_show = true;
            }
          }
        })
    },

    //第三排（右）
    init_bom_right () {
      this.$axios.get('/yiiapi/alert/list-top5')
        .then((resp) => {
          // console.log(resp)
          let {
            status,
            data
          } = resp.data;
          if (status == 0) {
            if (!isSynthetical(data)) {
              return false;
            } else {
              this.bom_right = data;
              this.bom_right_show = true;
            }
          }
        })
    },

    sys_state () {
      // this.$store.commit("CHANGE_SYS", true);
      this.el_dialog = true;
      this.get_data()
    },
    get_data () {
      this.equipment.probe = []
      this.equipment.engine = []
      this.equipment.engine_probe = []
      this.equipment.sandbox = []
      this.equipment.management = []
      this.equipment.management_probe = []
      this.equipment.echart_array = []
      this.equipment.links_array = []

      //   probe: [],
      // engine: [],
      // engine_probe: [],
      // sandbox: [],
      // management: [],
      // management_probe: [],
      this.$axios.get('/yiiapi/alert/system-state')
        .then(response => {
          let {
            status,
            data
          } = response.data;
          console.log(data);
          this.sysState_data = data
          data.dev_info.forEach(element => {
            //       { name: '引擎', type: "2" },
            // { name: '沙箱', type: "4" },
            // { name: '沙箱', type: "3" },
            // { name: '探针', type: "1" },
            // 1探针 2引擎 3引擎(探针) 4沙箱 5管理平台 6管理平台(探针)
            switch (element.type) {
              case '1':
                this.equipment.probe.push(element)
                break;
              case '2':
                this.equipment.engine.push(element)
                break;
              case '3':
                this.equipment.engine_probe.push(element)
                break;
              case '4':
                this.equipment.sandbox.push(element)
                break;
              case '5':
                this.equipment.management.push(element)
                break;
              case '6':
                this.equipment.management_probe.push(element)
                break;
              default:
                break;
            }
          });
          setTimeout(() => {
            this.graph_echart()
          }, 100);
        })
        .catch(error => {
          console.log(error);
        })
    },
    // 拓扑图
    graph_echart () {
      // 探针
      if (this.equipment.probe.length != 0) {
        this.equipment.probe.forEach((item, index) => {
          var data_item1 = {
            name: '探针' + index,
            names: '探针',
            dev_name: item.name,
            dev_ip: item.ip,
            status: item.status,
            next_ip: item.next_ip,
            symbolSize: 30,
            draggable: false,
            category: 0,
            symbol: 'image://' + ImagesIcon.probe,
            label: {
              normal: {
                position: "bottom",
                show: true,
                textStyle: {
                  fontSize: 12,
                  color: '#666',
                  align: 'center',
                },
                formatter: '探针'
              }
            },
            itemStyle: {
              normal: {

              }
            }
          };
          this.equipment.echart_array.push(data_item1);
        });
      }
      // 引擎
      if (this.equipment.engine.length != 0) {
        this.equipment.engine.forEach((item, index) => {
          var data_item2 = {
            name: '引擎' + index,
            names: '引擎',
            dev_name: item.name,
            dev_ip: item.ip,
            status: item.status,
            next_ip: item.next_ip,
            symbolSize: 30,
            draggable: false,
            category: 0,
            symbol: 'image://' + ImagesIcon.engine,
            label: {
              normal: {
                position: "bottom",
                show: true,
                textStyle: {
                  fontSize: 12,
                  color: '#666',
                  align: 'center',
                },
                formatter: '引擎'
              }
            },
            itemStyle: {
              normal: {

              }
            }
          };
          this.equipment.echart_array.push(data_item2);
        });
      }
      // 引擎/探针
      if (this.equipment.engine_probe.length != 0) {
        this.equipment.engine_probe.forEach((item, index) => {
          var data_item3 = {
            name: '引擎/探针' + index,
            names: '引擎/探针',
            dev_name: item.name,
            dev_ip: item.ip,
            status: item.status,
            next_ip: item.next_ip,
            symbolSize: 30,
            draggable: false,
            category: 0,
            symbol: 'image://' + ImagesIcon.engineProbe,
            label: {
              normal: {
                position: "bottom",
                show: true,
                textStyle: {
                  fontSize: 12,
                  color: '#666',
                  align: 'center',
                },
                formatter: '引擎/探针'
              }
            },
            itemStyle: {
              normal: {

              }
            }
          };
          this.equipment.echart_array.push(data_item3);
        });
      }
      // 沙箱
      if (this.equipment.sandbox.length != 0) {
        this.equipment.sandbox.forEach((item, index) => {
          var data_item1 = {
            name: '沙箱' + index,
            names: '沙箱',
            dev_name: item.name,
            dev_ip: item.ip,
            status: item.status,
            next_ip: item.next_ip,
            symbolSize: 30,
            draggable: false,
            category: 0,
            symbol: 'image://' + ImagesIcon.sandbox,
            label: {
              normal: {
                position: "bottom",
                show: true,
                textStyle: {
                  fontSize: 12,
                  color: '#666',
                  align: 'center',
                },
                formatter: '沙箱'
              }
            },
            itemStyle: {
              normal: {
              }
            }
          };
          this.equipment.echart_array.push(data_item1);
        })
      }
      // 管理平台
      if (this.equipment.management.length != 0) {
        this.equipment.management.forEach((item, index) => {
          var data_item5 = {
            name: '管理平台' + index,
            names: '管理平台引擎',
            dev_name: item.name,
            dev_ip: item.ip,
            status: item.status,
            next_ip: item.next_ip,
            symbolSize: 40,
            draggable: false,
            category: 0,
            symbol: 'image://' + ImagesIcon.management,
            label: {
              normal: {
                position: "bottom",
                show: true,
                textStyle: {
                  fontSize: 12,
                  color: '#666',
                  align: 'center',
                },
                formatter: '管理平台引擎'
              }
            },
            itemStyle: {
              normal: {
              }
            }
          };
          this.equipment.echart_array.push(data_item5);
        })
      }
      // 管理平台探针
      if (this.equipment.management_probe.length != 0) {
        this.equipment.management_probe.forEach((item, index) => {
          var data_item6 = {
            name: '管理平台探针' + index,
            names: '管理平台引擎(探针)',
            dev_name: item.name,
            dev_ip: item.ip,
            next_ip: item.next_ip,
            status: item.status,
            symbolSize: 40,
            draggable: false,
            category: 0,
            symbol: 'image://' + ImagesIcon.managementProbe,
            label: {
              normal: {
                position: "bottom",
                show: true,
                textStyle: {
                  fontSize: 12,
                  color: '#666',
                  align: 'center',
                },
                formatter: '管理平台引擎(探针)'
              }
            },
            itemStyle: {
              normal: {
              }
            }
          };
          this.equipment.echart_array.push(data_item6);
        })
      }
      var links = this.equipment.echart_array.filter(item => item.next_ip != '' && item.next_ip != null);
      console.log(links);
      links.forEach(ele => {
        var obj = {
          source: ele.name,
          target: '',
        }
        console.log(ele.next_ip);
        var targetArr = this.equipment.echart_array.filter(item => item.dev_ip == ele.next_ip)
        console.log(targetArr);
        obj.target = targetArr[0].name
        this.equipment.links_array.push(obj)
      })
      console.log(this.equipment.links_array);

      var myChart = this.$echarts.init(document.getElementById("graph"));
      // console.log(myChart);

      var option = {
        tooltip: {
          trigger: 'item',
          formatter: function (params, trigger) {
            // console.log(params);
            // console.log(trigger);
            if (params.dataType == 'node') {
              return '设备：' + params.data.dev_name + '</br>' + 'IP地址：' + params.data.dev_ip + '</br>' + '状态：' + params.data.status
            } else {
              return ''
            }
          }
        },
        animationDurationUpdate: 1500,
        animationEasingUpdate: 'quinticInOut',
        // color: ['#83e0ff', '#45f5ce', '#b158ff'],
        series: [{
          type: 'graph',
          layout: 'force',
          force: {
            repulsion: 300,
            edgeLength: 80
          },
          draggable: true,
          roam: true,
          label: {
            normal: {
              show: false
            }
          },
          data: this.equipment.echart_array,
          links: this.equipment.links_array,
          focusNodeAdjacenc: false, //是否在鼠标移到节点上的时候突出显示节点以及节点的边和邻接节点
          lineStyle: {
            normal: {
              color: '#F2F2F2',
              opacity: 0.9,
              width: 2,
              // curveness: 0
            },
          },
        }]
      }
      myChart.setOption(option);
      //添加点击事件
      myChart.off("click"); //防止累计触发
      myChart.on('click', (params) => {
        // console.log(params.data);
        //console.log(this.state_detail);
        this.state_detail = true;
        this.iot_detail_top(params.data)
      });

    },
    iot_detail_top (params) {
      this.equipment_detail.cpu = []
      this.equipment_detail.mem = []
      this.equipment_detail.disk = []
      this.equipment_detail.statistics_time = []
      this.equipment_detail.flow = []
      this.equipment_detail.title.type = params.names
      this.equipment_detail.title.ip = params.dev_ip
      this.equipment_detail.title.name = params.dev_name
      this.loading = true;

      this.$axios.get('/yiiapi/alert/dev-state', {
        params: {
          ip: params.dev_ip
        }
      })
        .then(response => {
          this.loading = false;
          let {
            status,
            data
          } = response.data;

          console.log('&&**')
          console.log(data);
          data.forEach(element => {
            this.equipment_detail.cpu.unshift(element.cpu)
            this.equipment_detail.mem.unshift(element.mem)
            this.equipment_detail.disk.unshift(element.disk)
            this.equipment_detail.statistics_time.unshift(element.statistics_time)
            this.equipment_detail.flow.unshift(element.flow)
          });
          this.equipment_detail.show = true;
          setTimeout(() => {
            this.cpu()
            this.flow()
          }, 100);

        })
        .catch(error => {
          console.log(error);
        })
    },
    closed_sys_box () {
      this.el_dialog = false;
    },
    closed_detail () {
      this.equipment_detail.show = false;
    },
    cpu () {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("cpu"));
      // 绘制图表
      myChart.setOption({
        grid: {
          top: "18%",
          left: '5%',
          right: "3%",
          bottom: "20%",
          containLabel: true
        },
        legend: {
          bottom: 5,
          left: 'center',
          orient: 'horizontal',
          textStyle: {
            fontSize: 12
          },
          selected: {
            // 选中'系列1'
            'CPU': true,
            '内存': true,
            '硬盘': true,
          },
          data: ['CPU', '内存', '硬盘']
        },
        tooltip: {
          trigger: "axis",
          borderColor: "rgba(2,136,209,0.3)",
          borderWidth: 2,
          backgroundColor: "#fff",
          textStyle: {
            color: "#ccc",
            align: 'left'
          },
          axisPointer: {
            lineStyle: {
              color: "#ccc"
            }
          }
        },
        xAxis: {
          boundaryGap: false,
          //网格样式
          splitLine: {
            show: true,
            lineStyle: {
              color: ["#F4F4F4"],
              width: 1,
              type: "solid"
            }
          },
          axisLine: {
            lineStyle: {
              color: "#ECECEC",
              width: 2
            }
          },
          axisLabel: {
            textStyle: {
              color: "#666666"
            }
          },
          axisTick: {
            show: false
          },
          data: this.equipment_detail.statistics_time,
        },
        yAxis: [{
          name: '单位(%)',
          nameTextStyle: {
            color: '#666'
          },
          splitLine: {
            show: true,
            lineStyle: {
              color: "#F4F4F4",
              width: 1,
              type: "solid"
            }
          },
          axisLine: {
            lineStyle: {
              color: "#ECECEC",
              width: 2
            }
          },
          axisLabel: {
            textStyle: {
              color: "#666666"
            }
          },
          axisTick: {
            show: false
          }
        }],
        color: ["rgba(2,136,209,0.9)", "rgba(205,220,57,0.9)", "rgba(76,175,80,0.9)"],
        visualMap: [{
          show: false,
          type: 'piecewise',
          seriesIndex: 0,
          pieces: [{
            gt: 85,
            color: '#dc5f5f'
          }, {
            gt: 0,
            lte: 85,
            color: "rgba(2,136,209,0.9)"
          }]
        }, {
          show: false,
          type: 'piecewise',
          seriesIndex: 1,
          pieces: [{
            gt: 85,
            color: '#dc5f5f'
          }, {
            gt: 0,
            lte: 85,
            color: "rgba(205,220,57,0.9)"
          }]
        }, {
          show: false,
          type: 'piecewise',
          seriesIndex: 2,
          pieces: [{
            gt: 80,
            color: '#dc5f5f'
          }, {
            gt: 0,
            lte: 80,
            color: "rgba(76,175,80,0.9)"
          }]
        }],
        series: [
          {
            name: "CPU",
            type: "line",
            symbol: "none",
            cursor: "pointer",
            smooth: true,
            data: this.equipment_detail.cpu,
            lineStyle: {
              color: "rgba(2,136,209,0.9)"
            },
            areaStyle: {
              color: {
                type: "linear",
                x: 0,
                y: 0,
                x2: 0,
                y2: 1,
                colorStops: [
                  {
                    offset: 0,
                    color: "rgba(2,136,209,0.3)" // 0% 处的颜色
                  },
                  {
                    offset: 1,
                    color: "rgba(2,136,209,0.1)" // 100% 处的颜色
                  }
                ]
              }
            }
          },
          {
            name: "内存",
            type: "line",
            symbol: "none",
            cursor: "pointer",
            smooth: true,
            data: this.equipment_detail.mem,
            lineStyle: {
              color: "rgba(205,220,57,0.9)"
            },
            areaStyle: {
              color: {
                type: "linear",
                x: 0,
                y: 0,
                x2: 0,
                y2: 1,
                colorStops: [
                  {
                    offset: 0,
                    color: "rgba(205,220,57,0.3)" // 0% 处的颜色
                  },
                  {
                    offset: 1,
                    color: "rgba(205,220,57,0.1)" // 100% 处的颜色
                  }
                ]
              }
            }
          },
          {
            name: "硬盘",
            type: "line",
            symbol: "none",
            cursor: "pointer",
            smooth: true,
            data: this.equipment_detail.disk,
            lineStyle: {
              color: "rgba(76,175,80,0.9)"
            },
            areaStyle: {
              color: {
                type: "linear",
                x: 0,
                y: 0,
                x2: 0,
                y2: 1,
                colorStops: [
                  {
                    offset: 0,
                    color: "rgba(76,175,80,0.3)" // 0% 处的颜色
                  },
                  {
                    offset: 1,
                    color: "rgba(76,175,80,0.1)" // 100% 处的颜色
                  }
                ]
              }
            }
          },
        ]
      });
      window.addEventListener("resize", () => {
        myChart.resize();
      });
    },
    flow () {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("flow_echarts"));
      // 绘制图表
      myChart.setOption({
        grid: {
          top: "18%",
          left: '5%',
          right: "3%",
          bottom: "20%",
          containLabel: true
        },
        legend: {
          bottom: 5,
          left: 'center',
          orient: 'horizontal',
          textStyle: {
            fontSize: 12
          },
          selected: {
            // 选中'系列1'
            '流量': true,
          },
          data: ['流量']
        },
        tooltip: {
          trigger: "axis",
          borderColor: "rgba(187,120,247,0.3)",
          borderWidth: 2,
          backgroundColor: "#fff",
          textStyle: {
            color: "#ccc"
          },
          axisPointer: {
            lineStyle: {
              color: "#ccc"
            }
          }
        },
        color: ["rgba(187,120,247,0.9)"],
        xAxis: {
          boundaryGap: false,
          //网格样式
          splitLine: {
            show: true,
            lineStyle: {
              color: ["#F4F4F4"],
              width: 1,
              type: "solid"
            }
          },
          axisLine: {
            lineStyle: {
              color: "#ECECEC",
              width: 2
            }
          },
          axisLabel: {
            textStyle: {
              color: "#666666"
            }
          },
          axisTick: {
            show: false
          },
          data: this.equipment_detail.statistics_time
        },
        yAxis: [{
          name: '单位(Mbps)',
          nameTextStyle: {
            color: '#666'
          },
          splitLine: {
            show: true,
            lineStyle: {
              color: "#F4F4F4",
              width: 1,
              type: "solid"
            }
          },
          axisLine: {
            lineStyle: {
              color: "#ECECEC",
              width: 2
            }
          },
          axisLabel: {
            textStyle: {
              color: "#666666"
            }
          },
          axisTick: {
            show: false
          }
        }],
        series: [
          {
            name: "流量",
            type: "line",
            symbol: "none",
            cursor: "pointer",
            smooth: true,
            data: this.equipment_detail.flow,
            lineStyle: {
              color: "rgba(187,120,247,0.9)"
            },
            areaStyle: {
              color: {
                type: "linear",
                x: 0,
                y: 0,
                x2: 0,
                y2: 1,
                colorStops: [
                  {
                    offset: 0,
                    color: "rgba(187,120,247,0.3)" // 0% 处的颜色
                  },
                  {
                    offset: 1,
                    color: "rgba(187,120,247,0.1)" // 100% 处的颜色
                  }
                ]
              }
            }
          }
        ]
      });
      window.addEventListener("resize", () => {
        myChart.resize();
      });
    }
  },
  components: {
    topLeft,
    topMidFlow,
    topMidFile,
    topRight,

    midLeft,
    midMid,
    midRight,

    bomLeft,
    bomMid,
    bomRight,

    // sysMonitor
  },
};
</script>

<style scoped lang="less">
.home_overview {
  padding: 24px;
  .container {
    text-align: left;
    .container_item {
      .color1 {
        background: #f57c00;
      }
      .color2 {
        background: #0288d1;
      }
      .color3 {
        background: #47cad9;
      }
      .color4 {
        background: #dc5f5f;
      }
      .color5 {
        background: #cddc39;
      }
      .color6 {
        background: #4caf50;
      }
      .color7 {
        background: #47cad9;
      }
      .color8 {
        background: #e0c840;
      }
      margin-bottom: 10px;
      .top_item {
        background: #ffffff;
        height: 380px;
        border-radius: 4px;
        padding: 0 16px 20px 16px;
        .title {
          height: 60px;
          line-height: 60px;
          font-family: PingFangMedium;
          font-size: 18px;
          color: #333333;
          .title_left {
            float: left;
          }
          .title_right {
            float: right;
            font-size: 14px;
            color: #333333;
            .title_right_icon {
              margin-left: 12px;
              border-radius: 2px;
              height: 14px;
              width: 28px;
              vertical-align: unset;
              display: inline-block;
            }
          }
        }
        .legend {
          clear: both;
          height: 30px;
          text-align: left;
          font-size: 14px;
          .legend_icon {
            border-radius: 2px;
            height: 14px;
            width: 28px;
            vertical-align: middle;
            display: inline-block;
          }
          .legend_title {
            margin-right: 20px;
          }
        }
        // 第一个
        .top_left_content {
          height: 270px;
        }
        // 第二个
        .top_mid_content {
          height: 300px;
          .content_top {
            height: 150px;
          }
          .content_bom {
            height: 150px;
          }
        }
        // 第三个
        .top_right_content {
          height: 270px;
        }
        // 第四个
        .bom_left_content {
          height: 300px;
          .content_top {
            height: 300px;
          }
        }
        // 第五个
        .bom_mid_content {
          height: 270px;
        }
        // 第六个
        .bom_right_content {
          height: 300px;
        }
      }
    }
  }
}

.vm-move-threat {
  margin: 0;
  padding: 0 !important;
  /deep/ .common-table {
    width: 100%;
    font-family: PingFangMedium;
    /deep/ .el-table__header-wrapper {
      .el-table__header {
        thead.has-gutter {
          th {
            background: #f8f8f8;
            .cell {
              color: #333;
            }
          }
        }
      }
    }
  }
  /deep/ &.vm-move-threat-middle {
    .common-table {
      .el-table__header-wrapper {
        .el-table__header {
          th:nth-child(3) {
            /*.cell{
                          padding: 0!important;

                        }*/
          }
        }
      }
      .el-table__body-wrapper {
        .el-table__row {
          td:nth-child(3) {
            .cell {
              position: relative;
              height: 12px;
              .el-progress {
                .el-progress-bar {
                  vertical-align: super;
                  .el-progress-bar__outer {
                    height: 12px !important;
                    border-radius: 1px;
                    .el-progress-bar__inner {
                      border-radius: 1px;
                      background-color: #5389e0;
                    }
                  }
                }
              }
              /*.sc_index{
                            background: #5389E0;
                            position: absolute;
                            top: 0;
                            left: 0;
                            width: 40%;
                            height: 16px;
                          }*/
            }
          }
        }
      }
    }
  }
}
</style>

<style lang="less">
.home_overview {
  .sys_box {
    z-index: 9000 !important;
    .el-dialog {
      width: 842px;
      .el-dialog__header {
        display: none;
      }
      .el-dialog__body {
        width: 842px;
        height: 462px;
        padding: 30px;
        .closed_img {
          position: absolute;
          top: -18px;
          right: -18px;
          cursor: pointer;
          width: 46px;
          height: 46px;
        }
        .title {
          height: 24px;
          line-height: 24px;
          text-align: left;
          font-size: 0;
          .mask {
            width: 4px;
            height: 16px;
            display: inline-block;
            background: #0070ff;
            vertical-align: baseline;
          }
          .title_name {
            font-size: 16px;
            color: #333333;
            margin-left: 6px;
            display: inline-block;
            vertical-align: super;
            font-family: PingFangMedium;
          }
        }
        #graph {
          width: 100%;
          height: calc(100% - 24px);
          position: relative;
        }
      }
    }
  }
  .sys_detail {
    z-index: 99999 !important;
    .el-dialog {
      width: 840px;
      .el-dialog__header {
        display: none;
      }
      .el-dialog__body {
        width: 840px;
        padding: 30px;
        .closed_img {
          position: absolute;
          top: -18px;
          right: -18px;
          cursor: pointer;
          width: 46px;
          height: 46px;
        }
        .title {
          height: 24px;
          line-height: 24px;
          text-align: left;
          .title_name {
            font-size: 20px;
            color: #333333;
            line-height: 24px;
          }
          .mask {
            width: 24px;
            height: 0px;
            border-top: 0px;
            border-right: 2px solid transparent;
            border-bottom: 5px solid #0070ff;
            border-left: 2px solid transparent;
            transform: rotate3d(0, 0, 1, 90deg);
            display: inline-block;
            margin-right: -5px;
            margin-bottom: 4px;
            margin-left: -10px;
          }
        }
        .sys_detail_content {
          text-align: left;
          .detail_item {
            margin-top: 24px;
            p {
              font-size: 14px;
              color: #999999;
            }
            #cpu,
            #flow_echarts {
              height: 200px;
            }
          }
        }
      }
    }
  }
}
.el-loading-mask {
  z-index: 99999 !important;
}
</style>
