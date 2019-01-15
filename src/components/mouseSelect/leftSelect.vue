<template>
    <div id="leftSelect">
        <div :class="isFixedHeader?'t-fixed-top':''">
          <slot name="header" >
            <Row class="t-font-size-13 t-text-center header">
                <Col span="2">序号</Col>
                <Col span="2"> 通道</Col>
                <Col span="3"> 频点（MHz）</Col>
                <Col span="2">译码同步</Col>
                <Col span="2">波形</Col>
                <Col span="2">速率(KBd)</Col>
                <Col span="2">误码率(BER</Col>
                <Col span="2">存盘</Col>
                <Col span="2">网络输出</Col>
                <Col span="2">控守</Col>
                <Col span="2">工作时长</Col>
            </Row>
          </slot>
        </div>
        <!-- :class="selectIndex.includes(item.name) ? activeClass : ''" @click.stop='leftmenu(item.name)'  -->
        <div class="content" id="selectDom" ref="selectDom" @mousedown.stop="mouseDown($event)" @mousemove.stop="mouseMove($event)" @mouseup.stop="mouseUp($event)">
              <slot name="list">
                <div v-for="(item, key) in mock" :data-val='item.name' :key="key" :class="childClass">
                    <Row  class="t-font-size-13 t-text-center " >
                        <Col span="2">
                            {{key}}
                        </Col>
                        <Col span="2">
                            <Select size="small" class="t-text-info t-padded-b-3">
                                <Option value="1" >通道1</Option>
                                <Option value="12">通道2</Option>
                                <Option value="13">通道3</Option>
                            </Select>
                        </Col>
                        <Col span="3">
                            {{key + 23}}MHz
                        </Col>
                        <Col span="2">
                            <span class="ivu-tag-dot-inner ivu-tag-success"></span>
                        </Col>
                        <Col span="2">
                            {{item.val}}
                        </Col>
                        <Col span="2">
                            64
                        </Col>
                        <Col span="2">
                            3.5E-5
                        </Col>
                        <Col span="2">
                            <i-switch v-model="scitch" size="small" />
                        </Col>
                        <Col span="2">
                            <i-switch size="small" />
                        </Col>
                        <Col span="2">
                            <i-switch size="small" />
                        </Col>
                        <Col span="2">
                            20:100
                        </Col>
                    </Row>
                </div>
              </slot>
        </div>
    </div>
</template>
<script>
  export default {
    name: 'leftSelect',
    props: {
      isFixedHeader: {
        type: Boolean,
        default: true
      },
      childClass: {
        type: String,
        default: 'item'
      },
      fields: {
        type: String,
        default: 'data-val'
      },
      activeClass: {
        type: String,
        default: 'active'
      }
    },
    data () {
        return {
          selectIndex: [],
          scitch: true,
          mouse: {
              selecting: false,
              start: {},
              end: {}
          },
          mock: [{
            name: 'demo1',
            val: '1'
          },
          {
            name: 'demo2',
            val: '2'
          },
          {
            name: 'demo3',
            val: '3'
          },
          {
            name: 'demo4',
            val: '4'
          },
          {
            name: 'demo5',
            val: '5'
          },
          {
            name: 'demo6',
            val: '6'
          },
          {
            name: 'demo7',
            val: '7'
          }]
        }
    },
    mounted() {
    },
    methods: {
        mouseDown(e) {
            this.mouse.selecting = true
            this.mouse.start = {
                left: e.x,
                top: e.y
            }
        },
        mouseMove(e) {
            // 开始遍历循环选中的 效果
            if (this.mouse.selecting) {
                    this.mouse.end = {
                        left: e.x,
                        top: e.y
                    }
                    let start = this.mouse.start.top
                    let end = this.mouse.end.top
                    let arr = []
                    let parent = this.$refs.selectDom
                    let dom = parent.getElementsByClassName(this.childClass)
                    for (let i = 0; i < dom.length; i++) {
                        this.selectIndex = []
                        // console.info(dom[i])
                        let thistop = dom[i].getBoundingClientRect().top + dom[i].getBoundingClientRect().height / 2 // 计算高度要包含
                        dom[i].classList.remove(this.activeClass);
                        if (end > start) {
                            if (thistop > start && thistop < end) {
                                arr.push(dom[i].getAttribute(this.fields))
                                dom[i].classList.add(this.activeClass);
                            }
                        } else if (end < start) {
                             if (thistop > end && thistop < start) {
                                arr.push(dom[i].getAttribute(this.fields))
                                dom[i].classList.add('active');
                            }
                        }
                    }
                    this.selectIndex = arr
                }
        },
        mouseUp(e) { // 结束
            this.mouse.selecting = false
            this.mouse.start = this.mouse.start = {}
            this.$emit('on-select', this.selectIndex);
        }
    }
  }
</script>
<style lang="less" scoped>
    @firstbcolor: #252323;
    @headercolor: #1E222D;
    @thirdbcolor: #2a2f3d;
    @theme: #2d8cf0;
    @color: #666;
    @cddd: #ddd;
    @c999: #999;
    #leftSelect{
        height: 100%;
        background-color: @headercolor;
        .header{
            padding: 5px 0 6px;
            border-bottom: 1px solid @firstbcolor;
            // background-color: @headercolor;
            color: @c999;
        }
        .content{
            height: 100%;
            overflow-y: auto;
            .item{
                line-height: 34px;
                height: 34px;
                // padding: 0 0 5px;
                color: @cddd;
            }
            .item.active{
                background-color: @theme !important;
                border-bottom: 1px solid @theme;
                .ivu-switch-checked{
                    border:1px #ddd solid
                }
            }
            .item:nth-child(even) {
                background-color: @thirdbcolor;
            }
        }
    }
</style>
<style lang="less" global>
    @inputbcolor: #252323;
    @theme: #2d8cf0;
    @color: #666;
    #leftSelect{
        .ivu-select-selection{
            background-color: @inputbcolor;
            border:0;
        }
        .ivu-form-item-label{
            color: @color;
        }
        .ivu-input-wrapper{
            .ivu-input{
                background-color:@inputbcolor;
                border:0;
            }
        }
    }
</style>
