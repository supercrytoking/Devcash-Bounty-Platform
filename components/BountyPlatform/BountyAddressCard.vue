<template>
  <div
    class="w-full flex flex-row flex-wrap justify-center rounded-xl px-4 py-2 shadow-xl border border-c-text-10"
  >
    <div class="flex flex-col justify-center items-center mx-3 mt-3 mb-4">
      <!-- Bounty Address Text and Copy Button  -->
      <div class="flex flex-row flex-wrap items-center">
        <p class="font-bold mr-2">{{ $t("bountyPlatform.singleBounty.contribute.bountyAddress") }}</p>
        <button
          @click.prevent="copyAddress()"
          class="w-8 h-8 rounded-full hover:bg-c-text-15 focus:bg-c-text-15 p-1 transition-colors duration-200"
        >
          <Icon colorClass="text-c-text" class="w-full h-full" type="copy" />
        </button>
      </div>
      <!-- Avatar and Address -->
      <div class="flex flex-row items-center mt-2">
        <Jazzicon class="flex" :diameter="48" :address="address" />
        <a class="hover:underline" target="_blank" :href="'https://etherscan.io/address/'+address">
          <p v-html="threeLineAddress" class="font-mono-jet text-sm ml-3"></p>
        </a>
      </div>
    </div>
    <!-- QR Code -->
    <div
      class="flex flex-row justify-center h-32 w-32 rounded-lg overflow-hidden border-3 border-c-primary my-4 mx-1"
    >
      <vue-qr
        :text="qrValue"
        :size="400"
        :logoScale="0.3"
        :logoSize="40"
        :correctLevel="2"
        :logoMargin="0"
        :logoBackgroundColor="'rgba(255, 255, 255, 0)'"
        :logoSrc="require('~/assets/images/icons/eth-avatar.svg')"
      ></vue-qr>
    </div>
  </div>
</template>
<script>
import Jazzicon from "~/components/Jazzicon.vue";
import Icon from "~/components/Icon.vue";
import Vue from "vue";
import VueClipboard from "vue-clipboard2";
VueClipboard.config.autoSetContainer = true; // add this line
Vue.use(VueClipboard);
export default {
  components: {
    Jazzicon,
    Icon
  },
  props: {
    address: String,
    qrValue: String
  },
  methods: {
    copyAddress() {
      this.$copyText(this.address);
      this.$notify({
          group: 'main',
          title: this.$t("bountyPlatform.singleBounty.contribute.headerAddressCopied"),
          text: this.$t("bountyPlatform.singleBounty.contribute.paragraphAddressCopiedBounty"),
          data: {},
          duration: 1500
        });
    }
  },
  computed: {
    threeLineAddress() {
      return (
        this.address.substring(0, 14) +
        "<br>" +
        this.address.substring(14, 28) +
        "<br>" +
        this.address.substring(28, 42)
      );
    },
  }
};
</script>
