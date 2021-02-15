<!--  -->
<template>
  <div>
    <my-table
      ref="myTable"
      :config="config"
      :tableData="vlans"
      @selectChange="selectChange"
    >
      <template v-slot:enable="{ row, prop }">
        <el-switch v-model="row[prop]"></el-switch>
      </template>
      <template v-slot:ether="{ row, prop }">
        <span @click="showLink(row[prop])">{{ getShowEther(row[prop]) }}</span>
      </template>
      <template v-slot:operation="{ row }">
        <el-button>更新</el-button>
        <el-button @click="delVlan(row['id'])">删除</el-button>
      </template>
    </my-table>

    <el-dialog title="显示vlans" :visible.sync="dialogTableVisible">
      <showVlans :ethers="ethers"></showVlans>
    </el-dialog>
  </div>
</template>
  
<script>
export default {
  components: {
    myTable: () => import("./components/myTable"),
    showVlans: () => import("./components/showEther"),
  },
  data() {
    return {
      config: {
        thead: [
          {
            prop: "vlan",
            lable: "vlan",
            type: "function",
            callback(row, prop) {
              var vlan = row[prop];
              row.vlanId = vlan.match(/(\d+)/)[0];
              return vlan[0].toUpperCase() + vlan.slice(1);
            },
          },
          { prop: "vlanId", lable: "vlanId" },
          { prop: "ether", lable: "以太物理口", type: "slot" },
          { prop: "mtu", lable: "mtu" },
          { prop: "enable", lable: "使能", type: "slot" },
          { lable: "操作", prop: "operation", type: "slot" },
        ],
        pageSizes: [2, 3, 4],
        url: "/api/vlan",
        //showSearch:false
      },
      vlans: [
        {
          id: "1",
          vlan: "vlan1",
          mtu: 1,
          enable: true,
          ether:
            "ether1,ether2,ether3,ether4,ether5,ether6,ether7,ether8,ether9,ether10,ether11,ether12",
        },
        {
          id: "2",
          vlan: "vlan22",
          mtu: 2,
          enable: false,
          ether: "ether1,ether2,ether3,ether4,ether5,ether6",
        },
        {
          id: "3",
          vlan: "vlan3",
          mtu: 2,
          enable: false,
          ether: "ether1,ether2,ether3,ether4,ether5,ether6",
        },
        {
          id: "4",
          vlan: "vlan4",
          mtu: 2,
          enable: false,
          ether: "ether1,ether2,ether3,ether4,ether5,ether6",
        },
      ],
      dialogTableVisible: false,
      ethers: [],
    };
  },
  methods: {
    selectChange(value) {
      this.selectedValue = value;
      console.log(this.selectedValue);
    },
    delVlan(id) {
      this.$confirm("确认删除此信息", "提示", {
        type: "warning",
      }).then(() => {
        this.data.forEach((vlan, idx) => {
          if (vlan.id === id) {
            this.data.splice(idx, 1);
          }
        });
        this.data.vlans = [];
        this.data.vlans.push.apply(this.data.vlans, this.getVlansList);
        console.log(this.data.vlans);
        this.$message({
          message: "删除成功",
          type: "success",
        });
      });
    },
    getVlansList() {
      return this.vlans;
    },
    showLink(ethers) {
      console.log(ethers);
      this.ethers = ethers.split(",");
      this.dialogTableVisible = true;
    },
    getShowEther(ethers) {
      return ethers.slice(0, 13) + "...";
    },
  },
  //生命周期 - 挂载完成（访问DOM元素）
  mounted() {
    this.data = this.getVlansList();
  },
};
</script>
<style scoped>
/* @import url(); 引入css类 */
</style>
