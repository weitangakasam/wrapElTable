# element-ui table的二次封装

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

封装的思路:

1.首先传入表格的 config,里面有 thead,pagesize(每一页显示的数量),pageSIzes([2,3,4]),
showSearch,mutiSelect,selectChange 对应的事件名称,这样点击多选之后,可以通过上面的删除按钮把
选中的数据删除掉
等一些跟表格属性的东西然后子组件里面接收到之后先遍历 config,然后把传入的 config 通过 key 来覆盖掉原本
里面有 table_config 的值

2.其中 config 的 thead 总有 table 中每个 el-table-column 所对应的 prop 也就是对应的 data 属性,还有 table 还有
type 等属性,其中 type 包括了 3 种自定义模式,分别是'function','slot'和默认也就是可以不填,
如果填了 function 的话,那么在 callback 回调函数中会收到两个参数分别是 row 和 prop,prop 对应的就是这一列的属性,
row 是这一行的数据,来源于 el-table-column 中的 scoped-slot="scoped",然后传入 scroped.row 即可
例如显示 vlanId 的时候就是返回正则匹配的数字.

3.然后当如果是 slot 的话,那么处理方式就是插槽,我们可以通过在<my-table></my-table>中传入<tempalte v-slot:propName="{row,prop}">
来获取值和 prop,但是这个 propName 必须和 type 是 slot 里面的 prop 要相对应才可以.然后在这里面我们可以插入修改删除按钮并对应的进行处理

4.然后在mytable组件中就可以使用v-for循环thead,渲染出每个表头列

5.最终显示的数据是 computed 的出来,默认是全部数据,一旦我们在搜索框中输入了内容,那么就需要设置字段,然后重新触发 computed


