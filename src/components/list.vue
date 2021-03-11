<template>
  <div class="max_div">
    <div class="left_div">
      <div class="block">
        <span class="demonstration">归属部门:</span>
        <el-cascader
          ref="cascader"
          size="mini"
          style="width: 200px;"
          v-model="value"
          :options="options"
          :props="{ expandTrigger: 'hover', label: 'name', value: 'id', emitPath: false }"
          @change="handleChange"></el-cascader>
      </div>
      <div class="block">
        <span class="demonstration">岗位名称:</span>
        <el-input v-model="workInfo.name" size="mini" placeholder="请输入内容"></el-input>
      </div>
      <div class="block filesDiv">
        <span class="demonstration">已选角色:</span>
        <div class="list_div">
          <li class="list_div_li" v-for="(item, index) of multipleSelection" :key="index">
            <i class="el-icon-check"></i> {{item.name}}
          </li>
        </div>
      </div>
    </div>
    <div class="right_div">
      <span class="demonstration">可授权的角色:</span>
      <el-table
        ref="multipleTable"
        :data="tableData"
        default-expand-all
        tooltip-effect="dark"
        style="width: 100%;height: 100%;border-radius: 5px;"
        row-key="id"
        :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
        @selection-change="handleSelectionChange">
        <el-table-column type="selection" width="55"></el-table-column>
        <el-table-column label="名称" width="180">
          <template slot-scope="scope">
            {{ scope.row.name }}
          </template>
        </el-table-column>
        <el-table-column prop="description" label="描述" show-overflow-tooltip>
        </el-table-column>
      </el-table>
    </div>
    <div style="margin-top: 20px;clear: both;padding-top: 40px">
      <el-button style="float:right;" type="primary" @click="submit()">保存</el-button>
    </div>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        value: 31, // 默认选择
        workInfo: {}, // 岗位
        options: [], // 部门
        tableData: [{
          id: 1,
          name: "中控系统",
          description: "",
          children: [],
        }, {
          id: 13,
          name: '文件服务',
          description: '',
          children: [],
        }, {
          id: 9,
          name: '客户关系管理',
          description: '',
          children: [],
        }],
        multipleSelection: [],
        rolerows: [],
        def:[],
      };
    },
    created(){
      this.getData();
    },
    methods: {
      getData(){
        this.$http({
            method: 'get',
            url: 'https://rest.apizza.net/mock/0ae54bb2f4f01b984a5c265147e48e3f/member/mod',
        }).then(res =>{
          this.options = res.data.depttree;
          this.rolerows = res.data.rolerows;
          this.workInfo = {
            id: res.data.data.deptid,
            name: res.data.data.position
          }
          let data = this.rolerows;
          let roleids = res.data.data.roleids;
          for(let item of data){
            for(let items of this.tableData){
              if(item.prodid == items.id && item.id != items.id){
                items.children.push({
                  description: item.description,
                  id: item.id,
                  name: item.name,
                });
              }
            }
          }
          // let roleids = res.data.data.roleids;
          // let chooseArr = [];
          // for(let item of this.rolerows){
          //   if(roleids.indexOf(item.id) != -1){
          //     chooseArr.push({
          //       description: item.description,
          //       hasChildren: false,
          //       id: item.id,
          //       name: item.name,
          //     })
          //   }
          // }
          // this.multipleSelection = chooseArr;
          this.getList(res.data.data.roleids);
        })
      },
      getList(roleids){
        for(let key in this.tableData){
          if(roleids.indexOf(this.tableData[key].id) != -1){
              this.$refs.multipleTable.toggleRowSelection(this.tableData[key])
          }
          for(let k in this.tableData[key].children){
            if(roleids.indexOf(this.tableData[key].children[k].id) != -1){
              this.$refs.multipleTable.toggleRowSelection(this.tableData[key].children[k])
            }
          }
        }
        
        // console.log(this.tableData[1].children[1],this.multipleSelection[1])
        // this.multipleSelection.forEach(row => {
        //   console.log(row)
        //   this.$refs.multipleTable.toggleRowSelection(row);
        // });
        // this.$refs.multipleTable.toggleRowSelection(this.tableData[1].children[1])
      },
      handleChange(value) {
        let chooseObj = this.$refs['cascader'].getCheckedNodes()[0].data;
        this.workInfo = chooseObj;
      },
      handleSelectionChange(val) {
        console.log(val,111)
        this.multipleSelection = val;
      },
      submit(){
      }
    }
  };
</script>
<style scoped lang="less">
.left_div{
  width: 200px;
  height: calc(100% - 100px);
  margin-right: 20px;
  float: left;
}
.right_div{
  width: calc(100% - 220px);
  height: calc(100% - 100px);
  float: left;
}
.filesDiv{
  height:calc(100% - 120px);
}
.list_div{
  background-color: #fff;
  border-radius: 5px;
  padding: 5px 10px;
  width: 180px;
  font-size: 14px;
  height:calc(100% - 10px);;
}
.list_div_li{
  list-style: none;
  padding: 5px;
  color: #4d3ae6;
  font-weight: 900;
}
</style>