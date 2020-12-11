<template>
   <transition name="modal">
      <div class="modal-mask">
         <div class="modal-wrapper">
            <div class="modal-container">
               <div class="modal-header">
                  Новый клиент успешно создан!
               </div>
               <div class="modal-body">{{ name }} {{ lastName }} спасибо за регистрацию!</div>
               <div class="justify-flex-end d-flex">
                  <button @click="$emit('close')">
                     OK
                  </button>
               </div>
            </div>
         </div>
      </div>
   </transition>
</template>

<script>
export default {
   name: 'registered',
   props: ['lastName', 'name', 'close'],
   data: () => ({}),
};
</script>

<style lang="sass">
.modal-mask
  position: fixed
  z-index: 9998
  top: 0
  left: 0
  width: 100%
  height: 100%
  background-color: rgba(28, 28, 28, 0.95)
  display: table
  transition: opacity 0.3s ease
.modal-wrapper
  display: table-cell
  vertical-align: middle
.modal-container
  width: 300px
  margin: 0px auto
  padding: 30px
  background-color: #fff
  border-radius: 2px
  transition: all 0.3s ease
.modal-body
  margin: 20px 0
.modal-leave-active
  opacity: 0
.modal-enter .modal-container,
.modal-leave-active .modal-container
  -webkit-transform: scale(1.1)
  transform: scale(1.1)
</style>
