<template>
  <div>
    <div v-if="popup" class="modal">

    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      popup: false,
    }
  },
  mounted(){
    this.openPopup()
  },
  methods: {
    openPopup() {
      return this.popup === true;
    },
  },
}
</script>
<style>
.modal {
  position: absolute;
  z-index: 5;
  background-color: rgba(0, 0, 0, 0.3);
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
</style>
