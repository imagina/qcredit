<template>
  <div id="walletPage">
    <!--Page actions-->
    <div class="box box-auto-height q-mb-md">
      <page-actions icon="fas fa-wallet" :title="$tr('icredit.cms.label.wallet')" ref="pageActions"
                    :extra-actions="extraPageActions" @refresh="getSummary(true); $refs.payoutConfig.loadPage()"/>
    </div>
    <!--payout config-->
    <payout-config class="q-mb-md" ref="payoutConfig"/>
    <!--Amount Available-->
    <div class="row q-col-gutter-x-md">
      <!--Available-->
      <div class="col-6 col-sm-3">
        <div class="box box-auto-height q-mb-md bg-blue text-white">
          <div class="text-weight-bold">
            <q-icon name="fas fa-dollar-sign"/>
            {{ this.$tr('isite.cms.label.available') }}
          </div>
          {{ $trn(this.summary.amount) }}
        </div>
      </div>
      <!--in-->
      <div class="col-6 col-sm-3">
        <div class="box box-auto-height q-mb-md bg-green text-white">
          <div class="text-weight-bold">
            <q-icon name="fas fa-caret-up"/>
            {{ this.$tr('isite.cms.label.inbound') }}
          </div>
          {{ $trn(this.summary.amountIn) }}
        </div>
      </div>
      <!--Out-->
      <div class="col-6 col-sm-3">
        <div class="box box-auto-height q-mb-md bg-red-4 text-white">
          <div class="text-weight-bold">
            <q-icon name="fa-solid fa-caret-down"/>
            {{ this.$tr('isite.cms.label.outbound') }}
          </div>
          {{ $trn(this.summary.amountOut) }}
        </div>
      </div>
      <!--Pending-->
      <div class="col-6 col-sm-3">
        <div class="box box-auto-height q-mb-md bg-amber text-white">
          <div class="text-weight-bold">
            <q-icon name="fas fa-history"/>
            {{ this.$tr('isite.cms.label.pending') }}
          </div>
          {{ $trn(this.summary.amountPending) }}
        </div>
      </div>
    </div>
    <!--Credits-->
    <crud :crud-data="import('modules/qcredit/_crud/wallet')"/>
    <!--Modal to withdrawal-->
    <master-modal v-model="modal.show" v-bind="modalConfig.props">
      <div class="box box-auto-height">
        <!--Amount Field-->
        <dynamic-field v-model="modal.amount" :field="modalConfig.amountField"
                       v-if="modalConfig.amountAvailable"/>
        <!--Message not amount-->
        <div v-else class="text-red-4">
          {{ $tr('icredit.cms.message.notAmountAvailable') }}...
        </div>
      </div>
    </master-modal>
  </div>
</template>

<script>
//Components
import payoutConfig from 'modules/qcommerce/_components/payout'

export default {
  props: {},
  components: {payoutConfig},
  watch: {},
  mounted() {
    this.$nextTick(function () {
      this.init()
    })
  },
  data() {
    return {
      summary: {amount: 0, amountIn: 0, amountOut: 0, amountPending: 0},
      customerId: this.$store.state.quserAuth.userId,
      modal: {
        loading: false,
        show: false,
        amount: null
      }
    }
  },
  computed: {
    //Extra Page actions
    extraPageActions() {
      return [
        {
          props: {
            label: this.$tr(`icredit.cms.label.withdrawal`),
            rounded: true,
            round: false,
            color: 'green',
            padding: '3px 15px'
          },
          action: () => {
            this.modal.show = true
          }
        },
      ]
    },
    //Modal props
    modalConfig() {
      //Cantidad disponible
      let amountAvailable = parseFloat(this.summary.amount) > 0 ? true : false

      return {
        amountAvailable: amountAvailable,
        props: {
          title: this.$tr(`icredit.cms.label.withdrawal`),
          loading: this.modal.loading,
          actions: !amountAvailable ? [] : [
            {
              props: {
                label: this.$tr('isite.cms.label.send'),
                color: 'green'
              },
              action: this.withdraw
            },
          ]
        },
        amountField: {
          value: null,
          type: 'input',
          props: {
            label: this.$tr('isite.cms.label.amount'),
            type: 'number'
          }
        }
      }
    }
  },
  methods: {
    //init
    async init() {
      this.getSummary()
    },
    //Get Amount available
    getSummary(refresh = false) {
      return new Promise(resolve => {
        //Request params
        let requestParams = {
          refresh: refresh,
          params: {filter: {customerId: this.customerId, amountAvailable: true}}
        }
        //Request
        this.$crud.index('/icredit/v1/credits', requestParams).then(response => {
          this.summary = response.data.length ? response.data[0] : {
            amount: 0,
            amountIn: 0,
            amountOut: 0,
            amountPending: 0
          }
          resolve(resolve.data)
        }).catch(err => {
          this.$apiResponse.handleError(error, () => {
            resolve(false)
          })
        })
      })
    },
    //withdraw
    withdraw() {
      let amountAvailable = parseFloat(this.summary.amount)
      //Validate Value
      if (this.modal.amount < 0 || this.modal.amount > amountAvailable) {
        this.$alert.warning(this.$tr('icredit.cms.message.invalidAmount'))
      } else {
        this.modal.loading = true
        //Request data
        let requestData = {
          amount: this.modal.amount
        }
        //Request
        this.$crud.create('apiRoutes.qcredit.withdrawalFunds', requestData).then(response => {
          this.$alert.info({message: `${this.$tr('isite.cms.message.recordCreated')}`})
          this.modal = {loading: false, show: false, amount: null}
          this.$refs.pageActions.emitRefresh()
        }).catch(error => {
          this.modal.loading = false
        })
      }
    }
  }
}
</script>

<style scoped>

</style>
