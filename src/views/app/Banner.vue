<template lang="pug">
#admin-banners.admin
  .admin-header
    .title
      h1 {{$route.meta.title}}
      el-button(type='text', @click="addBanner", v-show="listData.banners.length < Object.keys(positions).length") 添加Banner
  el-table(:data='listData.banners', border)
    el-table-column(prop='position', label='位置')
      template(slot-scope='scope')
        span {{positions[scope.row.position]}}
    el-table-column(prop='cover_url', label='图片', width='300')
      template(slot-scope='scope')
        img(:src='scope.row.cover_url', style='width: 100%')
    el-table-column(prop='model', label='类型')
      template(slot-scope='scope')
        span {{scope.row.banner_type === 'internal' ? scope.row.model : '外部链接'}}
    el-table-column(prop='state', label='状态')
    el-table-column(label='操作')
        template(slot-scope='scope')
          //- el-button(type='text',
                    @click='handleEdit(scope.$index, scope.row)') 编辑
          el-button(type='text',
                    @click='handleDestroy(scope.$index, scope.row, listData.banners)') 删除
</template>

<script>
import Base from '../base'
import api from 'stores/api'
const url = 'admin/banners'

const vm = Base({
  url: url,
  data: {
    positions: {},
    listData: {
      banners: []
    }
  },
  methods: {
    addBanner () {
      this.$router.push('/appbanner/new')
    },
    handleEdit (index, row) {
      this.$router.push(`appbanner/new?id=${row.id}`)
    }
  }
})
vm.mounted = function () {
  api.get(`${url}/app_positions`)
  .then((result) => {
    this.positions = result.data
  }).catch((err) => {
    this.$message.error(err.toString())
  })
}
export default vm

</script>

<style lang="stylus">
.active-ad
  background-color #e2f0e4 !important

</style>
