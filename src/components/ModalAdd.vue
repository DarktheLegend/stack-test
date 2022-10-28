<template>
  <div class="position-absolute start-0 end-0 top-0 bottom-0 d-flex justify-content-center align-items-center bg-black" style="--bs-bg-opacity: .25;">
    <div class="modal-add border border-2 border-success rounded-2">
      <div class="bg-success text-light text-center py-2 px-1">Добавить организацию</div>
      <div class="modal-add-body p-2">
        <input v-model="name" class="form-control py-1 my-1" placeholder="Название"/>
        <input v-model="phone" class="form-control py-1 my-1" placeholder="Номер телефона"/>
        <input v-model="director" class="form-control py-1 my-1" placeholder="ФИО директора"/>
      </div>
      <div class="bg-success text-light text-center p-1 d-flex">
        <button class="btn btn-light text-secondary mx-4" @click="$emit('cancel')">Отмена</button>
        <div class="flex-grow-1"></div>
        <button type="button" class="btn btn-light mx-4" @click="$emit('add-org', name, phone, director)" :class="{disabled: !valid}">Ок</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ModalAdd",
  data() {
    return {
      name: '',
      phone: '',
      director: ''
    }
  },
  emits: [
      'add-org',
      'cancel'
  ],
  computed: {
    valid() {
      return this.name.length > 0 && this.phone.length > 0 && this.director.length > 0;
    }
  }
}
</script>

<style scoped>
.modal-add {
  width: 300px;
}
.modal-add-body {
  background: #ecfff6;
}
</style>