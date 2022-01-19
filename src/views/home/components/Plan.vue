<template>
  <el-card shadow="hover">
    <template #header>
      <el-tag class="card-header-tag" type="success">列表拖拽</el-tag>
    </template>
    <el-table :data="tableData" row-key="title"  max-height="350px">
      <el-table-column align="center" label="拖拽" width="50px">
        <template #default="{}">
          <i class="el-icon-rank" style="cursor: pointer"></i>
        </template>
      </el-table-column>
      <el-table-column width="20px" />
      <el-table-column prop="title" label="目标" min-width="230px" />
      <el-table-column label="进度" width="220px">
        <template #default="{ row }">
          <el-progress :percentage="row.percentage" :color="row.color" />
        </template>
      </el-table-column>
      <el-table-column width="50px" />
      <el-table-column prop="endTIme" label="完成时间" />
    </el-table>
  </el-card>
</template>
<script>
  import Sortable from 'sortablejs'
  export default {
    data() {
      return {
        tableData: [
          {
            title: '昨夜雨疏风骤，浓睡不消残酒。试问卷帘人，却道海棠依旧。知否，知否？应是绿肥红瘦。',
            endTIme: '2099-12-31',
            percentage: 50,
            color: '#95de64',
          },
          {
            title: '薄雾浓云愁永昼，瑞脑销金兽。佳节又重阳，玉枕纱厨，半夜凉初透。',
            endTIme: '2029-12-31',
            percentage: 8,
            color: '#69c0ff',
          },
          {
            title: '东篱把酒黄昏后，有暗香盈袖。莫道不销魂，帘卷西风，人比黄花瘦。',
            endTIme: '2021-12-31',
            percentage: 76,
            color: '#1890FF',
          },
          {
            title: '黯乡魂，追旅思，夜夜除非，好梦留人睡。明月楼高休独倚，酒入愁肠，化作相思泪。',
            endTIme: '2020-03-31',
            percentage: 100,
            color: '#ffc069',
          },
          {
            title: '碧云天，黄叶地，秋色连波，波上寒烟翠。山映斜阳天接水，芳草无情，更在斜阳外。',
            endTIme: '2094-12-16',
            percentage: 25,
            color: '#5cdbd3',
          },
          {
            title: '世事一场大梦，人生几度秋凉？夜来风叶已鸣廊。看取眉头鬓上。酒贱常愁客少，月明多被云妨。中秋谁与共孤光。把盏凄然北望',
            endTIme: '此生无望',
            percentage: 1,
            color: '#b37feb',
          },
        ],
      }
    },
    mounted() {
      const tbody = document.querySelector('.el-table__body-wrapper tbody')
      const _this = this
      Sortable.create(tbody, {
        onEnd({ newIndex, oldIndex }) {
          const currRow = _this.tableData.splice(oldIndex, 1)[0]
          _this.tableData.splice(newIndex, 0, currRow)
        },
      })
    },
  }
</script>
<style scoped lang="less">
/deep/.el-table--scrollable-y ::-webkit-scrollbar {
  display: none;
}
</style>