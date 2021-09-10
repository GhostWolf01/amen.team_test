<template>
  <div class="table__item">
    <div class="table__item__content"
      v-if="item.name === 'statuses'"
    >
      <p class="table__item__statuses"
        :class="{ 
          'table__item__statuses--draft': itemStatuses.id === 1,
          'table__item__statuses--scheduled': itemStatuses.id === 2,
          'table__item__statuses--inactive': itemStatuses.id === 3,
          'table__item__statuses--active': itemStatuses.id === 4,
          'table__item__statuses--completed': itemStatuses.id === 5,
          'table__item__statuses--winner-notified': itemStatuses.id === 6
        }"
        v-for="itemStatuses in item.value"
        :key="itemStatuses.id"
      >
        {{ itemStatuses.name }}
      </p>
    </div>
    <div class="table__item__content"
      v-else-if="item.name === 'state'"
    >
      <button class="table__item__content__btn table__item__content__btn--accept"
        v-if="item.value === 'pending'"
        @click="accept"
      >
        Accept 
        <img class="table__item__content__btn-img"
          src="../assets/sucess.svg" 
          alt="Accept Img">
      </button>
      <button class="table__item__content__btn table__item__content__btn--decline"
        v-if="item.value === 'pending'"
        @click="decline"
      >
        Decline
        <img class="table__item__content__btn-img"
          src="../assets/x.svg" 
          alt="Decline Img">
      </button>
      <button class="table__item__content__btn table__item__content__btn--promote"
        v-if="item.value === 'accepted' && active"
        @click="promote"
      >
        Promote
        <img class="table__item__content__btn-img"
         src="../assets/Sweepstakes.svg" 
         alt="Promote Img">
      </button>
    </div>
    <p class="table__item__content"
      v-else-if="item.name.includes('date')"
    >
      {{ dateFilter() }}
    </p>
    <p class="table__item__content"
      v-else
    >
      {{ item.value }}
    </p>
  </div>
</template>

<script>
import moment from 'moment'

export default {
  name: 'TbaleItem',
  props: {
    item: { type: Object }
  },
  computed: {
    active() {
      for(const item of this.item.statuses){
        if(item.id === 4){
          return true
        }
      }
      return false
    }
  },
  methods: {
    accept() {
      this.$emit('accept', this.item.id)
    },
    promote() {
      this.$emit('promote', this.item.id)
    },
    decline() {
      this.$emit('decline', this.item.id)
    },
    dateFilter() {
      return moment(this.item.value).format("DD/MM/YYYY, hh:mm A")
    }
  },
}
</script>

<style lang="scss">
.table__item{
  min-width: 68px;
  margin-right: 30px;
  display: flex;
  align-items: center;
  &:last-of-type{
    margin-right: 10px;
  }
  &__content{
    display: flex;
    font-family: Inter;
    font-style: normal;
    font-weight: normal;
    font-size: 14px;
    line-height: 17px;
    color: #63517A;
    &__btn{
      cursor: pointer;
      width: 94px;
      height: 32px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 8px 16px;
      font-family: Inter;
      font-style: normal;
      font-weight: 500;
      font-size: 12px;
      line-height: 15px;
      border-radius: 35px;
      color: #FFFFFF;
      &--accept{
        background: #71B078;
        border: 1px solid #71B078;
        margin-right: 10px;
      }
      &--decline{
        background: #D3584B;
        border: 1px solid #D3584B;
      }
      &--promote{
        width: 100px;
        background: #6DB5D1;
        border: 1px solid #6DB5D1;
      }
    }
  }
  &__statuses{
    padding: 4px 8px;
    border-radius: 40px;
    font-family: Inter;
    font-style: normal;
    font-weight: 500;
    font-size: 12px;
    line-height: 15px;
    margin-right: 10px;
    &:last-of-type{
      margin-right: 0;
    }
    &--draft{
      border: 1px solid #9A83B7;
      color: #9A83B7;
      background: linear-gradient(0deg, rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.9)), #9A83B7;
    }
    &--scheduled{
      border: 1px solid #EDBC33;
      color: #EDBC33;
      background: linear-gradient(0deg, rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.9)), #EDBC33;
    }
    &--inactive{
      border: 1px solid #D3584B;
      color: #D3584B;
      background: linear-gradient(0deg, rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.9)), #D3584B;
    }
    &--active{
      border: 1px solid #71B078;
      color: #71B078;
      background: linear-gradient(0deg, rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.9)), #71B078;
    }
    &--completed{
      border: 1px solid #315B9C;
      color: #315B9C;
      background: linear-gradient(0deg, rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.9)), #315B9C;
    }
    &--winner-notified{
      border: 1px solid #6DB5D1;
      color: #6DB5D1;
      background: linear-gradient(0deg, rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.9)), #6DB5D1;
    }
  }
}
</style>
