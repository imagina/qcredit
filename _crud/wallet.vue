<template>
</template>
<script>
export default {
  data() {
    return {
      crudId: this.$uid(),
    }
  },
  computed: {
    crudData() {
      return {
        crudId: this.crudId,
        apiRoute: 'apiRoutes.qcredit.credits',
        create: false,
        read: {
          hideHeader: true,
          columns: [
            {name: 'id', label: this.$tr('isite.cms.form.id'), field: 'id', align: 'left'},
            {
              name: 'amount', label: this.$tr('isite.cms.label.amount'), field: 'amount', align: 'left',
              format: val => `${val.includes("-") ? '▼' : '▲'} ${this.$trn(val)}`,
              classes: row => row.amount.includes("-") ? "text-red-4" : "text-green"
            },
            {name: 'statusName', label: this.$tr('isite.cms.form.status'), field: 'statusName', align: 'left'},
            {
              name: 'description', label: this.$tr('isite.cms.form.description'), field: 'description',
              classes: 'ellipsis', style: 'max-width : 250px', format: val => val || '-', align: 'left'
            },
            {
              name: 'created_at', label: this.$tr('isite.cms.form.createdAt'), field: 'createdAt', align: 'right',
              format: val => val ? this.$trd(val, {type: 'long'}) : '-',
            },
            {
              name: 'updated_at', label: this.$tr('isite.cms.form.updatedAt'), field: 'updatedAt', align: 'right',
              format: val => val ? this.$trd(val, {type: 'long'}) : '-',
            },
          ],
          requestParams: {
            filter: {
              customerId: this.$store.state.quserAuth.userId
            }
          }
        },
        update: false,
        delete: false
      }
    }
  },
  methods: {}
}
</script>
