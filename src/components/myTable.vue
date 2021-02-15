<!--  -->
<template>
  <div>
    <div>
      <el-input
        style="float: right; width: 240px"
        v-if="table_config.showSearch"
        :value="searchPam"
        @input="search"
        size="mini"
        placeholder="输入关键字搜索"
      />
      <el-table
        :data="showData"
        style="width: 100%"
        @selection-change="handleSelectionChange"
      >
        <el-table-column
          v-if="table_config.multiSelect"
          type="selection"
        ></el-table-column>
        <!--这里外面用template包裹是因为把v-for和v-if写在一起性能比较差-->
        <template v-for="ele in table_config.thead">
          <el-table-column
            :key="ele.key"
            v-if="ele.type === 'function'"
            :prop="ele.prop"
            :label="ele.lable"
            width="180"
          >
            <template slot-scope="scoped">
              {{ ele.callback && ele.callback(scoped.row, ele.prop) }}
            </template>
          </el-table-column>
          <el-table-column
            v-else-if="ele.type === 'slot'"
            :key="ele.key"
            :prop="ele.prop"
            :label="ele.lable"
          >
            <template slot-scope="scope">
              <slot :name="ele.prop" :row="scope.row" :prop="ele.prop"></slot>
            </template>
          </el-table-column>
          <el-table-column
            v-else
            :key="ele.prop"
            :prop="ele.prop"
            :label="ele.lable"
          >
          </el-table-column>
        </template>
      </el-table>
      <el-pagination
        style="float: right"
        layout="sizes,prev, pager, next"
        @current-change="handleCurrentChange"
        @size-change="handleSizeChange"
        :page-size="table_config.pageSize"
        :page-sizes="table_config.pageSizes"
        :current-page.sync="currentPage"
        :total="data.length"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    config: {
      type: Object,
      default: () => {
        return {};
      },
    },
    tableData: {
      type: Array,
      default: () => {
        return [];
      },
    },
  },
  updated() {},
  created() {
    console.log(this.config);
    for (let key in this.config) {
      //也就是外面的参数把组件里面默认的给覆盖掉
      if (Object.keys(this.table_config).includes(key)) {
        this.table_config[key] = this.config[key];
      }
    }
    this.data = this.tableData;
  },
  data() {
    return {
      searchPam: "",
      currentPage: 1,
      table_config: {
        thead: [],
        showSearch: true,
        multiSelect: true,
        slectChange: "selectChange",
        pageSize: 2,
        pageSizes: [2],
        searchProp: "vlan",
      },
      data: [],
    };
  },
  computed: {
    showData() {
      let prevNum = this.table_config.pageSize * (this.currentPage - 1);
      console.log(prevNum);
      let data = this.data.slice(prevNum, prevNum + this.table_config.pageSize);
      if (this.searchPam.length === 0) {
        return data;
      } else {
        let searchProp = this.table_config.searchProp;
        let searchPam = this.searchPam;
        data = this.data.filter((ele) => {
          let reg = new RegExp(searchPam);
          return reg.test(ele[searchProp]);
        });
        console.log(data);
      }
      return data;
    },
  },
  methods: {
    search(value) {
      this.searchPam = value;
      this.currentPage = 1;
    },
    handleSelectionChange(value) {
      this.$emit("selectChange", value);
    },
    handleSizeChange(size) {
      this.table_config.pageSize = size;
    },
    handleCurrentChange(page) {
      this.currentPage = page;
    },
  },
};
</script>
<style scoped>
/* @import url(); 引入css类 */
</style>
