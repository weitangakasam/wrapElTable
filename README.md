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
1.首先传入表格的config,里面有thead,pagesize,showSearch,mutiSelect,selectChange对应的事件
等一些跟表格属性的东西然后子组件里面接收到之后先遍历config,然后把传入的config通过key来覆盖掉原本
里面有table_config的值

2.其中config的thead总有table中每个el-table-column所对应的prop也就是对应的data属性,还有table还有
type等属性,其中type包括了3种自定义模式,分别是'function','slot'和默认也就是可以不填,
如果填了function的话,那么在callback回调函数中会收到两个参数分别是row和prop,prop对应的就是这一列的属性,
row是这一行的数据,来源于el-table-column中的scoped-slot="scoped"
例如显示vlanId的时候就是返回正则匹配的数字.

3.然后当如果是slot的话,那么处理方式就是插槽,我们可以通过在<my-table></my-table>中传入<tempalte v-slot:propName="{row,prop}">
来获取值和prop,但是这个propName必须和type是slot里面的prop要相对应才可以.然后在这里面我们可以插入修改删除按钮并对应的进行处理

4.最终显示的数据是computed的出来,一旦我们在搜索框中输入了内容,那么就需要设置字段,然后重新触发computed
