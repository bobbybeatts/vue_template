<template>
  <div>
    <b-button
      v-ripple.400="'rgba(113, 102, 240, 0.15)'"
      style="float: right"
      variant="outline-primary"
      @click="searchOrderList"
    >
      재고처리/발주필요 목록조회
    </b-button>
    <b-button-group>
      <b-button
        v-ripple.400="'rgba(113, 102, 240, 0.15)'"
        variant="outline-primary"
        @click="OrderOpen"
      >
        모의재고처리 및 취합발주
      </b-button>
      <b-button
        v-ripple.400="'rgba(113, 102, 240, 0.15)'"
        variant="outline-primary"
        @click="OptionOrderOpen"
      >
        임의 발주
      </b-button>
      <b-button
        size="sm"
        variant="outline-primary"
        @click="selectAllRows"
      >
        Select all
      </b-button>
      <b-button
        size="sm"
        variant="outline-primary"
        @click="clearSelected"
      >
        Clear selected
      </b-button>

    </b-button-group>
    <div
      style="margin: 0 0 10px 0; float:right;"
    >
      <b-col>
        <b-form-group>
          <flat-pickr
            v-model="rangeDate"
            placeholder="시작일 & 종료일"
            class="form-control"
            :config="{ mode: 'range'}"
          />
        </b-form-group>
      </b-col>
    </div>
    <div>
      <b-table
        ref="selectableTable"
        :items="this.orderList"
        class="editable-table"
        hover
        selectable
        :select-mode="'multi'"
        :fields="fields"
        @row-selected="handleInput"
      />
    </div>
  </div>
</template>

<script>
import {
  BCol, BButtonGroup,
  BButton, BFormGroup, BTable, BFormInput,
} from 'bootstrap-vue'
import flatPickr from 'vue-flatpickr-component'
import Ripple from 'vue-ripple-directive'
import { orderList } from '@/components/logistic/outsourc/fields'
import ToastificationContent from '@core/components/toastification/ToastificationContent.vue'
import { mapActions, mapState } from 'vuex'

export default {
  name: 'Mps',
  components: {
    BCol,
    flatPickr,
    BFormGroup,
    BButtonGroup,
    BButton,
    BTable,
    BFormInput,
  },
  directives: {
    Ripple,
  },
  data() {
    return {
      rangeDate: null,
      startDate: null,
      endDate: null,
      fields: orderList,
    }
  },
  computed: {
    ...mapState('logi/order', ['orderList']),
  },
  methods: {
    ...mapActions('logi/order', ['SEARCH_ORDER_LIST']),
    selectAllRows() {
      this.$refs.selectableTable.selectAllRows()
    },
    clearSelected() {
      this.$refs.selectableTable.clearSelected()
    },
    extractDate() {
      this.startDate = this.rangeDate.split('to')[0].trim()
      this.endDate = this.rangeDate.split('to')[1].trim()
    },
    handleInput(payload) {
      this.contractDetail = payload
    },
    searchOrderList() {
      console.log('재고처리/발주필요 목록조회')
      if (this.rangeDate === null) {
        throw new Error('날짜입력')
      } else if (this.startDate === null || this.endDate === null) {
        throw new Error('날짜 범위 선택')
      } else {
        this.extractDate()
        const date = { startDate: this.startDate, endDate: this.endDate }
        this.SEARCH_ORDER_LIST(date)
      }
    },
    OrderOpen() {
      console.log('모의재고처리 및 취합발주')
    },
    OptionOrderOpen() {
      console.log('임의발주')
    },
  },
}
</script>
<style  lang="scss">
@import '@core/scss/vue/libs/vue-flatpicker.scss';
table.editable-table {
  margin: auto;
}

table.editable-table td {
  vertical-align: middle;
}

.editable-table .data-cell {
  padding: 8px;
  vertical-align: middle;
}

.editable-table .custom-checkbox {
  width: 50px;
}

.name-col {
  width: 120px;
}

.department-col {
  width: 150px;
}

.age-col {
  width: 100px;
}

.date-col {
  width: 200px;
}

.is-active-col {
  width: 100px
}
</style>
