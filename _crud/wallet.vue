<template></template>
<script>
export default {
  data() {
    return {
      crudId: this.$uid(),
      info: this.crudInfo
    }
  },
  mounted(){
  },
  computed: {

    crudData() {
      return {
        crudId: this.crudId,
        apiRoute: 'apiRoutes.qcredit.credits',
        read: {
          icon: 'fas fa-map-marker-alt',
          search: false,
          columns: [
            {
              name: 'id', label: this.$tr('ui.form.id'), field: 'id',
              align: 'left'
            },
            {
              name: 'amount', label: this.$tr('ui.label.amount'), field: 'amount',
              align: 'left'
            },
            {
              name: 'status', label: this.$tr('ui.form.status'), field: 'status',
              format: val => val || '-', align: 'left'
            },
            {
              name: 'date', label: this.$tr('ui.form.date'), field: 'date',
              align: 'left'
            },
            {
              name: 'description', label: this.$tr('ui.form.description'), field: 'description',
              classes: 'ellipsis', style: 'max-width : 250px', format: val => val || '-', align: 'left'
            },

            //{name: 'actions', label: this.$tr('ui.form.actions'), align: 'left'},
          ],
          requestParams: {filter: {customerId: this.$store.state.quserAuth.userId}}
        },

      }
    },
    //Crud info
    crudInfo() {


      /*
      var axios = require('axios');

      var config = {
        method: 'get',
        url: 'https://inmeet.ozonohosting.com/api/icredit/v1/credits?',
        headers: {
          'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJhdWQiOiIxIiwianRpIjoiZjE0NTdiNjMxZTdiZDQ2ZWViMjQ0YzFjZmMxNGVlYzA3YTc4OTNkMzk0ZjMwM2U3Mzg2Y2YxMmMyOTJlYjI3NGZiMTY0YTZlYjQxYTI5M2YiLCJpYXQiOjE2NDMwMzgwNjYsIm5iZiI6MTY0MzAzODA2NiwiZXhwIjoxNjc0NTc0MDY2LCJzdWIiOiIyMSIsInNjb3BlcyI6W119.WNUqO50MBEvc8EPNOo-Sp7PZ5cni5qUPxnAdT_pDIY6rtLkllrIBemFU6CUsnBR8TtH-uZ3UjinFJP7l7sLVBGDWYmURMOvvNpwEWYgZ0FrbdoCHa3LUmBTUPJFUyGl34STrWCyksWFfhyiEecsqd2jjqVAr1yHttNhK2QFHZ7o9_1bRLMhC2ypYvMFnQBDKtf9KGkMKVir_LZJ_kBCUUYol9cks_0oousBYGhp3zOFllo09jCRRLuH5Di38YLf81-xHJifWY3qZswgWNTC9AqwvYdYh_DU_XRmzcpBwshFwS6lIkhP_0Io-23eigj19lC0Qbh4dyszhRZW2vUnDyFpzeHlKeX5h2wGG18B5GKC4--LxSt6CHeQBX78uvD_PKMfeGNLojnMjRE4Wh6N1wFznPXAGZzA4kIM7NGb5IVghvdOzDxIQ1Ha0zMNnSSsgzYCqCHwdsbfblgSmsU2JEGCmaVLyfbgEh3EsEDO9RDHUFhoS3lWGZTLTxh_5eMcF-FB_r3Pa8NVO_yqxKlYv8BIbgan9K_unwYLwYxMZUfsvll4sDnE1VV1slcuneUV-MzyJEUAdeKiJ04_5WHZHC98Ek3fN4pcIkLDwqdborKwhWDsFB8Z2rsklM5I1fXXTgvqbvNYPxZ5Udkvbtq3QFjKWpWrV3BwfKrjPdRsHT2U',
          'Cookie': 'laravel_session=eyJpdiI6IlUrVWlUenJwcjAweVFySGYzckZwS0E9PSIsInZhbHVlIjoiNGdIWnM3N05vUWUvdWUxYlV5WE9paTBLN3lzY1cvRmdqRzNWa1B1cTFtcnQyaEU4eGZRbjlXbVpWYlk5TW5ra0dOYUw5RnZCRm1pc1VDS3lRQXdHVDFueDBJK3J0OUpGRDZ4UWhtSkg1c3RZQUJ3SFVHTGlZRmVRdFRMaDRCcjQiLCJtYWMiOiI0MmU3OTJkOGM2ZDlmNWE3OWZkZjY0NzY4ZWI2MDQ4MTA2OWFiOWMyOTQ3ZTliNmQ0ZTNiYzhhYTUyZDZlM2Y4IiwidGFnIjoiIn0%3D'
        }
      };

      axios(config)
          .then(function (response) {
            console.log((response.data.data));
            return JSON.stringify(response.data.data)
          })
          .catch(function (error) {
            return console.log(error);
          });*/
    }

  },
  methods: {}
}
</script>
