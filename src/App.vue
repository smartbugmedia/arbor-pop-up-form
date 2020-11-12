<template>
  <div class="cms-vue-boilerplate-container">
    <transition name="fade">
      <Modal :modalData="finalModal" @close="toggleModal" ref="modal" v-if="showModal" />
    </transition>
  </div>
</template>

<script>
import Modal from '@/components/Modal.vue';

export default {
  name: 'App',
  props: ['moduleData'],
  data: function() {
    return {
      showModal: false,
      potentialModals: this.moduleData.potentialModals
    };
  },
  computed: {
    finalModal() {
      return this.potentialModals[Math.floor(Math.random() * this.potentialModals.length)];
    }
  },
  methods: {
    refreshCookie() {
      if (!localStorage.getItem('modal_closed')) {
        this.showModal = true;
      }
    },
    toggleModal(e) {
      this.showModal = !this.showModal;
    }
  },
  created: function() {
    console.log(this.finalModal);
    if (this.finalModal.trigger == "time") {
      let seconds = this.finalModal.number_of_seconds * 1000;
      setTimeout(() => {
        this.toggleModal();
      }, seconds);
    } else {
      console.log("exit intent");
      document.addEventListener('mouseout', e => {
        // if (!e.toElement && !e.relatedTarget && !localStorage.getItem('modal_closed')) {
        //   console.log("This should have fired");
        //   this.toggleModal();
        //   localStorage.setItem('modal_closed', 'true');
        // }
        if (!e.toElement && !e.relatedTarget) {
          console.log("This should have fired");
          this.toggleModal();
          localStorage.setItem('modal_closed', 'true');
        }
      });
    }
  },
  components: {
    Modal
  },
};
</script>

<style lang="scss">
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
