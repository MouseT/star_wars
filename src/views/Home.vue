<template>
  <div class="home">
    <div class="home__content"
     :class="{blurred: isBlurred}"
    >
      <HomeHeader/>
      <HomeList
        @activateModal="activateModal"
      />
      <HomeFooter />
    </div>
    <HomeModal
      :isModalClicked="isModalClicked"
      :dataFromItem="dataFromItem"
      :loadingModal="loadingModal"
      @hideModal="hideModal()"
    />
  </div>

</template>

<script>
  import HomeHeader from "../components/Home/HomeHeader";
  import HomeList from "../components/Home/HomeList";
  import HomeFooter from "../components/Home/HomeFooter";
  import HomeModal from "../components/Home/HomeModal";

export default {
  name: 'Home',
  components: {
    HomeFooter,
    HomeList,
    HomeHeader,
    HomeModal
  },
  data: () => ({
    isBlurred: false,
    isModalClicked: false,
    dataFromItem: [],
    loadingModal: true
  }),
  methods: {
    activateModal: function (item) {
      this.isModalClicked = true;
      this.isBlurred = true;
      this.$store.dispatch('fetchDataModal', {species: item.species, homeworld: item.homeworld, films: item.films});
      let body = document.getElementById('body')
      body.classList.add('no-scroll')
      setTimeout(() => {
        this.dataFromItem = this.$store.getters.getDataModal;
        this.dataFromItem.push(item);
        setTimeout(() => {
          this.loadingModal = false;
        }, 1000)
      }, 1000)
    },
    hideModal: function () {
      this.isModalClicked = false;
      this.isBlurred = false;
      this.loadingModal = true;
      let body = document.getElementById('body')
      body.classList.remove('no-scroll')
    },
  },
}
</script>
<style lang="scss">
  .blurred {
    pointer-events: none;
    margin: 0px -25px;
    -webkit-filter: blur(14px);
    -moz-filter: blur(14px);
    -o-filter: blur(14px);
    -ms-filter: blur(14px);
    filter: blur(14px);
  }
  .no-scroll {
    height: 100%;
    width: 100%;
    position: fixed;
    overflow: hidden;
  }
</style>
