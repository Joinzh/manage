<template>
  <div style="height:100%">
    <Layout :style="{minHeight: '100%'}">
      <Layout>
        <Header>
          <Button class="top-btn" :class="{top_btn_active:tabIdx == 'eat'}" @click="pushName('eat')">堂食（08）</Button>
          <Button class="top-btn" :class="{top_btn_active:tabIdx == 'out-eat'}" @click="pushName('out-eat')">外卖（21）</Button>
          <Button class="top-btn" :class="{top_btn_active:tabIdx == 'reserve'}" @click="pushName('reserve')">预定（02）</Button>
          <div style="float:right;color: #fff; height: 100%;" v-show="tabIdx == 'eat'">
            <Checkbox
                :indeterminate="indeterminate"
                :value="checkAll"
                @click.prevent.native="handleCheckAll">全选</Checkbox>
            <CheckboxGroup v-model="checkAllGroup" @on-change="checkAllGroupChange">
              <Checkbox label="一层"></Checkbox>
              <Checkbox label="二层"></Checkbox>
              <Checkbox label="三层"></Checkbox>
            </CheckboxGroup>
          </div>
          <div style="float:right;color: #fff; height: 100%;" v-show="tabIdx == 'out-eat'">
            <CheckboxGroup v-model="fruit" @on-change="orders">
              <Checkbox label="自动接"></Checkbox>
              <Checkbox label="手动接"></Checkbox>
              <Checkbox label="不接单"></Checkbox>
            </CheckboxGroup>
          </div>
          <div style="float:right;color: #fff; height: 100%;" v-show="tabIdx == 'reserve'">
            <CheckboxGroup v-model="switchBtn" @on-change="reserves">
              <Checkbox label="开启"></Checkbox>
              <Checkbox label="关闭"></Checkbox>
            </CheckboxGroup>
          </div>
        </Header>
        <Content class="index_bg">
          <router-view></router-view>
        </Content>
      </Layout>
      <Sider style="min-width: 18vw;max-width: 18vw;" width="18vw" class="laout-rgt">
        <div class="rgt-item">
          <div class="shop_info">
            <div class="shop_item">
              <img class="shop_item_img" src="../../assets/vip.png" v-show="tabIdx == 'eat'" alt="">
              <img class="shop_item_img" src="../../assets/dan.png" v-show="tabIdx == 'out-eat'" alt="">
              <img class="shop_item_img" src="../../assets/yu.png" v-show="tabIdx == 'reserve'" alt="">
              <div class="shop_text" v-if="tabIdx == 'eat'">会员</div>
              <div class="shop_text" v-else-if="tabIdx == 'out-eat'">开单</div>
              <div class="shop_text" v-else-if="tabIdx == 'reserve'">预约</div>
            </div>
            
            <div class="shop_item">
              <img class="shop_item_img" src="../../assets/zhuo.png" alt="" v-show="tabIdx != 'out-eat'">
              <div class="shop_text" v-show="tabIdx != 'out-eat'">拼桌</div>
            </div>
            
            <div class="shop_item">
              <img class="shop_item_img" src="../../assets/kai.png" alt="">
              <div class="shop_text">开班</div>
            </div>
          </div>
        </div>

        <div class="rgt-item">
          <div class="btn-all">
            <Button class="itemBtn">加菜</Button>
            <Button class="itemBtn">加菜</Button>  
            <Button class="itemBtn">加菜</Button>
            <Button class="itemBtn">加菜</Button>
            <Button class="itemBtn">加菜</Button>  
            <Button class="itemBtn">加菜</Button>
            <Button class="itemBtn">加菜</Button>
            <Button class="itemBtn">加菜</Button>  
            <Button class="itemBtn">加菜</Button>
            <Button class="itemBtn">加菜</Button>
            <Button class="itemBtn">加菜</Button>  
            <Button class="itemBtn">加菜</Button>
            <Button class="itemBtn">加菜</Button>
            <Button class="itemBtn">加菜</Button>  
            <Button class="itemBtn">加菜</Button>
          </div>
        </div>
        <div class="order-deteile">
          <div class="order-top">
            <span style="display:inline-block;margin-top:10px;">
              订单详情
            </span>
          </div>
          <div class="order-title">订单10</div>
          <div class="order-num order_detele">订单编号：123412341241234</div>
          <div class="order-time order_detele">开单时间：2018-06-14 11:11:11</div>
          <div class="order-table order_detele">
            <Row class="orderList_table">
              <Col :span="6"><div class="item_name">商品</div></Col>
              <Col :span="6"><div class="item_price item_center">单价</div></Col>
              <Col :span="6"><div class="item_num item_center">数量</div></Col>
              <Col :span="6"><div class="item_state item_center">状态</div></Col>
            </Row>
            
            <Row v-for="(item,index) in orderList" :key="index" class="orderList_table">
              <Col :span="6">
                <div class="item_name">{{item.name}}</div>
              </Col>
              <Col :span="6">
                <div class="item_price item_center">{{item.price}}</div>
              </Col>
              <Col :span="6">
                <div class="item_num item_center">{{item.num}}</div>
              </Col>
              <Col :span="6">     
                <div class="item_state item_center">{{item.state == '2'? '退':''}}</div>
              </Col>                
            </Row>
            
          </div>
          <div class="all-order all_order">
            <Row>
              <Col :span="12"><div>总价：</div></Col>
              <Col :span="12"><div class="item_rgt">172.0</div></Col>
              <Col :span="12"><div>退菜：</div></Col>
              <Col :span="12"><div class="item_rgt">50.0</div></Col>
              <Col :span="12"><div>优免：</div></Col>
              <Col :span="12"><div class="item_rgt">5.0</div></Col>
            </Row>
          </div>
          
          <div class="all_order">
            <Row>
              <Col :span="12"><div>系统支付：</div></Col>
              <Col :span="12"><div class="item_rgt">115.0</div></Col>
              <Col :span="12"><div>未付款：</div></Col>
              <Col :span="12"><div class="item_rgt">2.0</div></Col>
            </Row>
          </div>
        </div>
      </Sider>
    </Layout>
  </div>
</template>
<script>
  // import util from '@/libs/util.js'
  export default {
    data () {
      return {
        tabIdx: '',
        indeterminate: true,
        checkAll: false,
        checkAllGroup: ['一层', '二层'],
        fruit: ['自动接'],
        switchBtn: ['开启'],
        orderList: [
          {
            name: '宫保鸡丁',
            price: '20.0',
            num: '1',
            state: '1'
          },
          {
            name: '鱼香肉丝',
            price: '35.0',
            num: '2',
            state: '2'
          },
          {
            name: '米饭',
            price: '2.0',
            num: '10',
            state: '1'
          },
          {
            name: '鲜榨橙汁',
            price: '28.0',
            num: '1',
            state: '1'
          }
        ]
      }
    },
    beforeMount:function() {
      this.onloadData();
    },
    methods: {
      onloadData () {
        this.tabIdx = this.$route.name
      },
      pushName(name,index){
        this.$router.push({
          name: name
        })
        this.tabIdx = name
      },
      handleCheckAll () {
        if (this.indeterminate) {
          this.checkAll = false;
        } else {
          this.checkAll = !this.checkAll;
        }
        this.indeterminate = false;

        if (this.checkAll) {
          this.checkAllGroup = ['一层', '二层', '三层'];
        } else {
          this.checkAllGroup = [];
        }
      },
      checkAllGroupChange (data) {
        if (data.length === 3) {
            this.indeterminate = false;
            this.checkAll = true;
        } else if (data.length > 0) {
            this.indeterminate = true;
            this.checkAll = false;
        } else {
            this.indeterminate = false;
            this.checkAll = false;
        }
      },
      // 外卖
      orders (e) {
        console.log(e)
        console.log(e[0])
        console.log(e.shift())
        if (e[0] == 'undefined') {
          console.log('1')
        }else {
          this.fruit.push(e.shift())
          console.log(this.fruit)
        }
      },
      // 预定开启
      reserves (e) {
        e.shift()
        this.switchBtn.push(e.shift())
      }
    },
  }
</script>
<style scoped>
  .ivu-layout-header {
    height: 70px;
  }
  .top-btn {
    width: 160px;
    height: 45px;
    margin-top: 13px;
    margin-right: 10px;
    border-radius: 7px;
    float: left;
    font-size: 15px;
    font-weight: bold;
    color: #000;
    text-align: center;
    border-color: #c4c8d4;    
    background: #c4c8d4;
    box-shadow:0px -3px 5px #999 inset;
  }
  .top-btn:hover {
    color: #000;
    background: #c4c8d4;
    border-color: #c4c8d4;    
  }
  .top_btn_active {
    color: #fff;
    background: #2abbae;
    border-color: #2abbae;
  }
  .top_btn_active:hover {
    color: #fff;
    background: #2abbae;  
    border-color: #2abbae;  
  }
  .ivu-checkbox-group {
    float: right;
  }
  .index_bg {
    height: calc(100vh - 70px);
    overflow: auto;
    background: linear-gradient(to bottom,  #94a0ba, #eccfa5);
  }

  /* 右边 */
  .rgt-item {
    width: 100%;
    float: left;
    background: #1e2538;
  }
  .shop_info {
    width: 100%;
    display: flex;
    justify-content: space-around;
    border-bottom: 1px solid #555;
  }
  .shop_item {
    height: 126px;
    flex: 1;
    box-sizing: border-box;
    padding-top: 30px;
  }
  .shop_item > .shop_item_img {
    width: 40px;
    display: block;
    margin: 0 auto;
  }
  .shop_item > .shop_text {
    text-align: center;
    color: #fff;
    margin-top: 10px;
  }
  /* 右边所有按钮 */
  .btn-all {
    box-sizing: border-box;
    padding: 14px 12px;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
  }
  .itemBtn {
    width: 32%;
    margin-top: 10px;
    background-color: #4d546e;
    border-color: rgba(255, 255, 255, 0);
    color: #fff;
    font-size: 15px;
  }
  /* 右边订单详情 */
  .order-deteile {
    width: 100%;
    height: 100%;
    background: #414c6a;
    color: #fff;
    padding: 0 10px;
    box-sizing: border-box;
  }
  .order-title {
    text-align: center;
    font-size: 22px;
    margin-top: 10px;
  }
  .order_detele {
    margin-top: 5px;
    font-size: 16px;
  }
  .item_center {
    text-align: center;
  }
  .all-order {
    margin-top: 140px;
  }
  .all_order {
    border-top: 1px dashed #fff;
    padding: 15px 0;
    font-size: 16px;
  }
  .item_rgt {
    text-align: right;
  }
</style>
