<script>
import NavBar from "@/components/NavBar.vue";
export default {
  name: "LayoutInitial",
  components: {
    NavBar,
  },
  props: {
    ShowText: {
      type: Boolean,
      default: true,
    },
  },
};
</script>
<template>
  <div class="layout-initial">
    <NavBar :ShowText="ShowText"></NavBar>
    <div class="content">
      <slot />
    </div>
  </div>
</template>
<style>
.layout-initial {
}

.content {
  margin-top: 20px;
  padding-bottom: 40px;
}
</style>
