<template>
  <div v-if="invoicesLoaded">
    <div v-if="!mobile" class="app flex">
      <Navigation />
      <div class="app-content flex flex-column">
        <Modal v-if="modalActive" />
        <transition name="invoice">
          <InvoiceModal v-if="invoiceModal" />
        </transition>

        <router-view />
      </div>
    </div>
    <div v-else class="mobile-message flex flex-column">
      <h2>Sorry, this app is not supported on Mobile Devices</h2>
      <p>To use this app, please use a computer or Tablet</p>
    </div>
  </div>
</template>

<script>
import { mapState, mapActions } from "vuex";
import InvoiceModal from "./components/InvoiceModal.vue";
import Navigation from "./components/Navigation.vue";
import Modal from "./components/Modal.vue";

export default {
  name: "app",
  data() {
    return {
      mobile: null,
    };
  },
  components: {
    Navigation,
    InvoiceModal,
    Modal,
  },
  created() {
    this.GET_INVOICES();
    this.checkScreen();
    window.addEventListener("resize", this.checkScreen);
  },
  methods: {
    ...mapActions(["GET_INVOICES"]),
    checkScreen() {
      const windowWidth = window.innerWidth;
      if (windowWidth <= 750) {
        this.mobile = true;
        return;
      }
      this.mobile = false;
    },
  },
  computed: {
    ...mapState(["invoiceModal", "modalActive", "invoicesLoaded"]),
  },
};
</script>


<style>
.invoice-enter-active,
.invoice-leave-active {
  transition: 0.8s ease all;
}

.invoice-enter-from,
.invoice-leave-to {
  transform: translateX(-700px);
}
</style>