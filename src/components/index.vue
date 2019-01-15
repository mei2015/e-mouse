<template>
  <div class="demo">
    <p class="t-text-left t-margin-t-20 t-margin-b-20">
    <img width="20" src="../assets/logo.png" style="vertical-align:-5px;display:inline"> 这个一个 electron-vue 左键长按选择插件，下面是一个简单例子：</p>
    <leftSelect @on-select='getBackVal'>
        <template slot='list'>
            <div v-for="(item, key) in mock" :data-val='item.name' :key="key" class="item" >
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
        </template>
    </leftSelect>
    <h2 class="t-text-left t-padded-t-20 t-padded-b-10">API</h2>
    <Table border :columns="api" :data="apidata"></Table>
    <h2 class="t-text-left t-padded-t-20 t-padded-b-10">Events</h2>
    <Table border :columns="events" :data="eventsdata"></Table>
    <h2 class="t-text-left t-padded-t-20 t-padded-b-10">Slot</h2>
    <Table border :columns="slot" :data="slotdata"></Table>
    <div class="t-padded-t-15 t-margin-t-10 t-font-size-13 t-text-left">备注：目前仅支持，一级下的多选，插销内选中效果，需自行实现，返回了选中后的值</div>
  </div>
</template>
<script>
export default {
    data () {
        return {
            selectIndex: [],
            scitch: true,
            haha2: '22222',
            api: [
                {
                    title: '属性',
                    key: 'title'
                },
                {
                    title: '说明',
                    key: 'sate'
                },
                {
                    title: '类型',
                    key: 'type'
                },
                {
                    title: '默认值',
                    key: 'default'
                }
            ],
            apidata: [
                {
                    title: ':fields',
                    sate: '绑定选中的字段',
                    type: 'String',
                    default: 'data-val'
                },
                {
                    title: ':childClass',
                    sate: 'list节点class，主要用于选中的class 效果',
                    type: 'String',
                    default: 'item'
                },
                {
                    title: ':activeClass',
                    sate: '选中效果class',
                    type: 'String',
                    default: 'active'
                }
            ],
            events: [
                {
                    title: '事件',
                    key: 'title'
                },
                {
                    title: '说明',
                    key: 'sate'
                },
                {
                    title: '返回值',
                    key: 'back'
                }
            ],
            eventsdata: [
                {
                    title: '@on-select',
                    sate: '选择后的返回函数',
                    back: 'item[]：已选项数据'
                }
            ],
            slot: [
                {
                    title: '名称',
                    key: 'title'
                },
                {
                    title: '说明',
                    key: 'sate'
                }
            ],
            slotdata: [
                {
                    title: 'header',
                    sate: '头部自定义设置'
                },
                {
                    title: 'list',
                    sate: '列表内容循环自定义'
                }
            ],
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
    created() {
        this.haha2 = '11111'
    },
    methods: {
        getBackVal(res) {
            this.selectIndex = res
            console.info('获取到返回值', res)
        }
    }
}
</script>
<style lang="less">
    @headercolor: #1E222D;
    @firstbcolor: #252323;
    @c999: #999;
    @theme: #2d8cf0;
    @cddd: #666;
    .demo{
      width: 900px;
      margin: 20px auto 0;
      padding-bottom: 20px;
      .header{
        padding: 5px 0 6px;
        border-bottom: 1px solid @firstbcolor;
        background-color: @headercolor;
        color: @c999;
      }
        .item{
            line-height: 34px;
            height: 34px;
            // padding: 0 0 5px;
            color: @cddd;
        }
        .item.active{
            background-color: @theme !important;
            border-bottom: 1px solid @theme;
        }
    }
</style>
