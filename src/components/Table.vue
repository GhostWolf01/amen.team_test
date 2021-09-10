<template>
  <div class="table">
    <div class="table__title">
      <h2 class="table__title-text">
        Sweepstakes
      </h2>
    </div>
    <div class="table__control">
      <div class="table__control__filter">
        <p class="table__control__filter-text">Filters:</p>
        <button class="table__control__filter__btn"
          v-for="(item, index) in dataFilter"
          :key="index"
          :class="{ 'table__control__filter__btn--active': item.active}"
          @click="filter(item)"
        >
          {{ `${item.countItemFilter} ${item.name}` }}
        </button>
      </div>
      <div class="table__control__pages">
        <p class="table__control__pages__showing">
          {{`SHOWING ${showingCount} OF ${showingOf}`}}
        </p>
        <div class="table__control__pages__rows">
          <p class="table__control__pages__rows-text">
            ROWS PER PAGE:
          </p>
          <div class="table__control__pages__rows__select__container">
            <select class="table__control__pages__rows__select"
              v-model="rowsCount"
              @change="changeRowsCount"
            >
              <option v-for="(item, index) in rowsPerPage"
                :key="index"
                :value="item">
                  {{item}}
              </option>
            </select>
              <svg class="table__control__pages__rows__select__arrow" 
                width="6" 
                height="9" 
                viewBox="0 0 6 9" 
                fill="none" 
                xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M0.96742 1.05031C0.674922 1.32714 0.674922 1.77432 0.96742 2.05114L3.8774 4.8052L0.967419 7.55925C0.674921 7.83608 0.674921 8.28326 0.967419 8.56008C1.25992 8.83691 1.73241 8.83691 2.02491 8.56008L5.46739 5.30206C5.75989 5.02524 5.75989 4.57806 5.46739 4.30123L2.02491 1.04321C1.73992 0.773487 1.25992 0.773487 0.96742 1.05031Z" fill="#A098AB"/>
              </svg>
          </div>
        </div>
        <div class="table__control__pages__page">
          <button class="table__control__pages__page-btn"
            @click="pagePrev"
          >
            <img class="table__control__pages__page-btn-svg table__control__pages__page-btn-svg--left"
              src="../assets/arrow.svg" 
              alt="arrow left">
          </button>
          <p class="table__control__pages__page-text">
            {{pageCount}}
          </p>
          <button class="table__control__pages__page-btn"
            @click="pageNext"
          >
            <img class="table__control__pages__page-btn-svg table__control__pages__page-btn-svg--right"
              src="../assets/arrow.svg" 
              alt="arrow right">
          </button>
        </div>
      </div>
    </div>
    <div class="table__data__container">
      <div class="table__data__container__scroll">
        <div class="table__data">
          <div class="table__data__header">
            <TableHeader v-for="(item, index) in tableHeader"
              :key="index"
              :item="item"
              @sortActive="sortActive"
            />
          </div>
          <div class="table__data__body">
            <div class="table__data__body__item"
              v-for="(itemTableData, index) in tableData"
              :key="index"
            >
              <TabaleItem v-for="(itemTableHeader, index) in tableHeader"
                :key="index"
                :item="{ 
                  id: itemTableData?.id ?? '',
                  value: itemTableData?.[itemTableHeader.value] ?? '',
                  name: itemTableHeader?.value ?? '',
                  statuses: itemTableData?.statuses ?? []
                }"
                @accept="acceptItem"
                @promote="promoteItem"
                @decline="declineItem"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
    <TablePromote ref="promote"/>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import TableHeader from './TableHeader.vue'
import TabaleItem from './TabaleItem.vue'
import TablePromote from './TablePromote.vue'

export default {
  name: 'Table',
  components: {
    TableHeader,
    TabaleItem,
    TablePromote
  },
  data() {
    return {
      dataFilter: [
        {
          idFilter: '',
          name: 'All',
          countItemFilter: '',
          active: true,
        },
        {
          idFilter: 4,
          name: 'Active',
          countItemFilter: '',
          active: false,
        },
        {
          idFilter: 3,
          name: 'Inactive',
          countItemFilter: '',
          active: false,
        },
        {
          idFilter: 5,
          name: 'Completed',
          countItemFilter: '',
          active: false,
        }
      ],
      rowsCount: 10,
      rowsPerPage: [
        3,
        5,
        10,
        15
      ],
      pageCount: 1,
      tableHeader: [
        {
          text: 'Title',
          value: 'title'
        },
        {
          text: 'Focus',
          value: 'focus'
        },
        {
          text: 'Raised',
          value: 'raised'
        },
        {
          text: 'Entries',
          value: 'entrants'
        },
        {
          text: 'Status',
          value: 'statuses'
        },
        {
          text: 'Actions',
          value: 'state'
        },
        {
          text: 'Start date, time',
          value: 'start_date'
        },
        {
          text: 'End date, time',
          value: 'end_date'
        }
      ],
      tableData: [],
      filterTableData: []
    }
  },
  computed: {
    ...mapGetters({
      getData: 'getData'
    }),
    showingCount() {
      if (this.rowsCount > this.filterTableData.length) {
        return this.filterTableData.length
      } else if (this.pageCount > 1) {
        if (this.rowsCount * this.pageCount > this.filterTableData.length) {
          return this.filterTableData.length - this.rowsCount
        }
      }
      return this.rowsCount
    },
    showingOf() {
      return this.filterTableData.length
    }
  },
  created() {
    this.countStatus()
    this.filterTableData = this.filterTableData.concat(this.getData)
    for(let i = 0; i < this.showingCount; i++){
      this.tableData.push(this.filterTableData[i])
    }
  },
  methods: {
    countStatus() {
      for(let i = 1; i < this.dataFilter.length; i++){
        this.dataFilter[i].countItemFilter = 0
      }

      for(const item of this.getData){  
        for(const itemStatuses of item.statuses){
          let seachSucess = false
          for(let i = 1; i < this.dataFilter.length; i++){
            if (this.dataFilter[i].idFilter === itemStatuses.id){
              this.dataFilter[i].countItemFilter++
              seachSucess = true
              break
            }
          }
          if(seachSucess) break
        }
      }
    },
    filter(itemDataFilter){
      for (const item of this.dataFilter){
        item.active = false
      }

      this.filterTableData = []
      if (itemDataFilter.idFilter) {
        for(const item of this.getData){  
          for(const itemStatuses of item.statuses){
            if(itemStatuses.id === itemDataFilter.idFilter){
              this.filterTableData.push(item)
              break;
            }
          }
        }
      } else {
        this.filterTableData = this.filterTableData.concat(this.getData)
      }

      this.changeRowsCount()
      itemDataFilter.active = true
    },
    sortActive(item){
      if (item.sort) {
        this.getData.sort((a, b) => {
          if( a[item.name] > b[item.name] ) return -1 
          if( a[item.name] === b[item.name] ) return 0 
          if (a[item.name] < b[item.name]) return 1
        })
      } else {
        this.getData.sort((a, b) => {
          if( a[item.name] > b[item.name] ) return 1 
          if( a[item.name] === b[item.name] ) return 0 
          if (a[item.name] < b[item.name]) return -1
        })
      }

      for(const item of this.dataFilter) {
        if ( item.active ) {
          this.filter(item)
          break
        }
      }
    },
    pagePrev(){
      if( this.pageCount-1 >= 1){
        this.pageCount--
        this.changeRowsCount()
      } 
    },
    pageNext() {
      if( this.pageCount+1 < this.showingOf/this.rowsCount + 1){
        this.pageCount++
        this.changeRowsCount()
      }
    },
    changeRowsCount() {
      this.tableData = []
      if (this.showingCount === this.filterTableData.length) this.pageCount = 1
      for(let i = 0; i < this.showingCount; i++){
        this.tableData.push(this.filterTableData[i + (this.pageCount - 1 ) * this.rowsCount])
      }
    },
    acceptItem(id){
      for(const item of this.tableData) {
        if(item.id === id){
          item.state = 'accepted'
          break;
        }
      }
    },
    promoteItem(id){
      this.$refs.promote.handleShow(id)
    },
    declineItem(id){
      for(let i = 0; i < this.getData.length; i++){
        if (this.getData[i].id === id) {
          this.getData.splice(i, 1)
          break;
        }
      }

      this.countStatus()

      for(const item of this.dataFilter) {
        if ( item.active ) {
          this.filter(item)
          break
        }
      }
    }
  },
}
</script>

<style lang="scss">
.table{
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  &__title{
    display: flex;
    margin-bottom: 30px;
    &-text{
      font-family: Inter;
      font-style: normal;
      font-weight: bold;
      font-size: 24px;
      line-height: 29px;
      color: #000000;
    }
  }
  &__control{
    display: flex;
    align-items: center;
    margin-bottom: 25px;
    &__filter{
      display: flex;
      align-items: center;
      &-text{
        font-family: Inter;
        font-style: normal;
        font-weight: bold;
        font-size: 14px;
        line-height: 17px;
        text-align: center;
        color: #303852;
      }
      &__btn{
        cursor: pointer;
        font-family: Inter;
        font-style: normal;
        font-weight: 600;
        font-size: 14px;
        line-height: 17px;
        text-align: center;
        color: #303852;
        background: #FFFFFF;
        transition: all 0.2s linear;
        border-radius: 20px;
        border: none;
        padding: 12.5px 16.5px;
        margin-left: 2px;
        transition: all 0.2s linear;
        &--active, &:hover{
          box-shadow: 0px 10px 10px rgba(48, 56, 82, 0.2);
        }
      }
    }
    &__pages{
      margin-left: 290px;
      display: flex;
      align-items: center;
      &__showing{
        font-family: Inter;
        font-style: normal;
        font-weight: normal;
        font-size: 12px;
        line-height: 15px;
        color: #A098AB;
        margin-right: 24px;
      }
      &__rows{
        display: flex;
        align-items: center;
        margin-right: 24px;
        &-text{
          font-family: Inter;
          font-style: normal;
          font-weight: normal;
          font-size: 12px;
          line-height: 15px;
          color: #A098AB;
          margin-right: 5px;
        }
        &__select{
          cursor: pointer;
          padding: 0px 8px 0px 16px;
          width: 60px;
          height: 25px;
          font-family: Inter;
          font-style: normal;
          font-weight: 600;
          font-size: 14px;
          line-height: 17px;
          color: #A098AB;
          background: #FFFFFF;
          border: 1px solid #A098AB;
          border-radius: 28px;
          -webkit-appearance: none;
          -moz-appearance: none;
          appearance: none;
          &:hover, &:focus{
            color: #a098bf;
            border: 1px solid #a098bf;
          }
          &__container{
            position: relative;
          }
          &__arrow{
            position: absolute;
            bottom: 8px;
            right: 12px;
            transform: rotate(-90deg);
          }
        }
      }
      &__page{
        display: flex;
        align-items: center;
        &-btn{
          cursor: pointer;
          width: 24px;
          height: 24px;
          background: #F2EEF9;
          border-radius: 28px;
          padding: 8px;
          display: flex;
          justify-content: center;
          align-items: center;
          border: none;
          &:hover{
            background: #F2EFFF;
            box-shadow: 0px 0px 5px rgba(48, 56, 82, 0.2);
          }
          &-svg{
            width: 5px;
            height: 8px;
            &--left{
              transform: rotate(180deg);
            }
          }
        }
        &-text{
          margin: 0 5px;
          font-family: Inter;
          font-style: normal;
          font-weight: bold;
          font-size: 16px;
          line-height: 16px;
          color: #63517A;
        }
      }
    }
  }
  &__data{
    width: max-content;
    display: flex;
    flex-direction: column;
    margin-bottom: 10px;
    &__container{
      width: 100%;
      padding: 20px;
      border: 1px solid #F2EEF9;
      border-radius: 10px;
      &__scroll{
        width: 100%;
        overflow: auto;
      }
    }
    &__header{
      display: grid;
      padding-left: 20px;
      grid-template-columns: 190px 170px 90px 80px 230px 230px 180px 170px; 
      margin-bottom: 20px;
    }
    &__body{
      display: flex;
      flex-direction: column;
      &__item{
        display: grid;
        grid-template-columns: 190px 170px 90px 80px 230px 230px 180px 170px; 
        background: #FFFFFF;
        border: 1px solid #F2EEF9;
        border-radius: 10px;
        margin-bottom: 10px;
        padding-left: 20px;
        height: 60px;
      }
    }
  }
}
</style>
