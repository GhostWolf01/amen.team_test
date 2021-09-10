<template>
  <div class="table__promote"
  v-if="show">
    <div class="table__promote__container">
      <button class="table__promote__close"
        @click="cancel"
      >
        <img class="table__promote__close-img"
          src="../assets/x_close.svg"
          alt="close">
      </button>
      <div class="table__promote__header">
        <p class="table__promote__header-text">
          Public shareable link
        </p>
      </div>
      <div class="table__promote__link">
        <a class="table__promote__link-text">
          {{ `uncommongood.com/sweepstakes/${id}` }}
        </a>
      </div>
      <div class="table__promote__footer">
        <button class="table__promote__footer-btn"
          @click="cancel"
        >
          Cancel
        </button>
        <button class="table__promote__footer-btn table__promote__footer-btn--copy"
          v-if="!saved"
          @click="copy"
        >
          <img class="table__promote__footer-btn-img"
            src="../assets/Link.svg"
            alt="link">
          Copy link
        </button>
        <button class="table__promote__footer-btn table__promote__footer-btn--saved"
          v-if="saved"
          @click="cancel"
        >
          Saved!
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TablePromote',
  data() {
    return {
      saved: false,
      show: false,
      id: ''
    }
  },
  methods: {
    handleShow(id) {
      this.id = id
      this.saved = false
      this.show = true
    },
    cancel() {
      this.show = false
    },
    copy() {
      navigator.clipboard.writeText(`uncommongood.com/sweepstakes/${this.id}`)
      this.saved = true
    }
  }
}
</script>

<style lang="scss">
.table__promote{
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba($color: #000000, $alpha: 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  &__close{
    cursor: pointer;
    position: absolute;
    top: 10px;
    right: 10px;
    width: 40px;
    height: 40px;
    display: flex;
    justify-content: center;
    align-items: center;
    border: 1px solid #F2EEF9;
    border-radius: 8px;
    background: none;
  }
  &__container{
    position: relative;
    background: #FFFBFB;
    width: 544px;
    border-radius: 16px;
    padding: 65px 30px 35px 30px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
  }
  &__header{
    width: 100%;
    margin-bottom: 25px;
    &-text{
      font-family: Inter;
      font-style: normal;
      font-weight: 800;
      font-size: 20px;
      line-height: 24px;
      text-align: center;
      color: #63517A;
    }
  }
  &__link{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 305px;
    height: 50px;
    background: #FFFFFF;
    border: 1px solid #EFEFEF;
    border-radius: 40px;
    padding: 10px 20px;
    margin-bottom: 35px;
    &-text{
      text-decoration: none;
      font-family: Inter;
      font-style: normal;
      font-weight: 600;
      font-size: 14px;
      line-height: 17px;
      color: #63517A;
    }
  }
  &__footer{
    display: flex;
    justify-content: center;
    align-items: center;
    &-btn{
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 140px;
      height: 50px;
      padding: 10px 16px;
      background: #F2EEF9;
      border-radius: 54px;
      border: none;
      font-family: Inter;
      font-style: normal;
      font-weight: 600;
      font-size: 14px;
      line-height: 17px;
      text-align: center;
      color: #63517A;
      &--copy{
        background: #EDBC33;
        box-shadow: 0px 6px 10px rgba(237, 188, 51, 0.3);
        border-radius: 30px;
        color: #FFFFFF;
        margin-left: 24px;
      }
      &--saved{
        background: #71B078;
        box-shadow: 0px 6px 10px rgba(113, 176, 120, 0.3);
        border-radius: 30px;
        color: #FFFFFF;
        margin-left: 24px;
      }
      &-img{
        margin-right: 10px;
      }
    }
  }
}
</style>