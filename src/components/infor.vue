<template>
  <div>
    <!--      <el-container v-for="item in infor" :key="main">-->
    <!--        {{item}}-->
    <!--      </el-container>-->
    <!--      <el-select v-model="value1" multiple placeholder="请选择">-->
    <!--        <el-option-->
    <!--          v-for="item in options"-->
    <!--          :key="item.value"-->
    <!--          :label="item.label"-->
    <!--          :value="item.value">-->
    <!--        </el-option>-->
    <!--      </el-select>-->

    <!--      <el-select-->
    <!--        v-model="value2"-->
    <!--        multiple-->
    <!--        collapse-tags-->
    <!--        style="margin-left: 20px;"-->
    <!--        placeholder="请选择">-->
    <!--        <el-option-->
    <!--          v-for="item in options"-->
    <!--          :key="item.value"-->
    <!--          :label="item.label"-->
    <!--          :value="item.value">-->
    <!--        </el-option>-->
    <!--      </el-select>-->
    <!--      <div>-->
    <!--        {{ this.newsList}}-->
    <!--      </div>-->
    <h1>欢迎进入系统</h1>
    <!--      <el-dialog-->
    <!--        title="编辑"-->
    <!--        :visible.sync="dialogVisible"-->
    <!--        width="35%"-->
    <!--        :before-close="handleClose">-->
    <!--        <el-form ref="form" :model="aj" label-width="80px">-->
    <!--          <el-form-item label="物品名">-->
    <!--            <span  >woqu</span>-->
    <!--          </el-form-item>-->
    <!--          <el-form-item>-->
    <!--            <el-button type="primary" >保存</el-button>-->
    <!--            <el-button @click="dialogVisible = false">取消</el-button>-->
    <!--          </el-form-item>-->
    <!--        </el-form>-->
    <!--      </el-dialog>-->
  </div>
</template>

<script>
  export default {
    data() {
      return {
        infor: [],
        options: [{
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
        value1: [],
        value2: [],
        newsList: '',
        jinjie: [],
        ajk: [],
        inid: '',
        dialogVisible:'',
        real:'',
        show:'',
        jin:''
      }
    },
    created() {
      this.getlist();
      this.getNewsList()
      this.getList()
      this.getjinjie()
      // this.open()
    },
    methods: {
      logout() {
        window.sessionStorage.clear();
        this.$router.push("/login");
      },
      async getlist() {
        const {data: res} = await this.http.post("/allinfor")
        this.infor = res
      },
      toggerCollapse() {
        this.isCollapse = !this.isCollapse
      },
      async nn() {
        const {data: res} = await this.http.post("/nn")
        this.nn = res
      },
      getNewsList() {
        this.axios.post('allinfor').then((response) => {
          this.newsList = response.data.data;
        }).catch((response) => {
        })
      },
      getList() {
        this.axios({
          url: 'allinfor',
          method: 'post'
        }).then(res => {
        })
      },
      async getjinjie() {
        const {data: res} = await this.http.post("/jinjie")
        this.jin = res.msg
        if(this.jin=="true"){
          await this.open()
        }
      // console.log(this.jinjie)
    },
    async open() {
      const {data: res} = await this.http.post("/jinjie")
      this.jinjie = res.data
      let dialogVisib
      for (let i = 0; i < this.jinjie.length; i++) {
        this.real=this.jinjie[i].inname
        this.inid = "[" + "物品名:" + this.jinjie[i].inname + "," + "货源:" + this.jinjie[i].incompany + "]"
        this.ajk.push(this.inid)

      }

        this.$alert(this.ajk + "大于设定最大库存", '标题名称', {
          confirmButtonText: '确定',
        });


    },
  }
  }

</script>

<style scoped>

</style>
