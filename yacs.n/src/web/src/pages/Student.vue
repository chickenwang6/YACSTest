<template>
  <div>
    <Header class="mb-3"></Header>
    <router-view />
    <Footer />
  </div>
</template>

<script>
import store from "@/store";
import { SELECT_SEMESTER } from "@/store";
import HeaderComponent from "@/components/Header";
import FooterComponent from "@/components/Footer";
import { userTypes } from "@/store/modules/user";

export default {
  name: "StudentPage",
  components: {
    Header: HeaderComponent,
    Footer: FooterComponent,
  },
  async beforeRouteEnter(to, from, next) {
    try {
      if (!store.getters[userTypes.getters.IS_LOGGED_IN]) {
        await store.dispatch(userTypes.actions.LOAD_SESSION_COOKIE);
      }
      // eslint-disable-next-line no-empty
    } catch {}

    if (store.state.selectedSemester === null || to.query.semester) {
      await store.dispatch(SELECT_SEMESTER, to.query.semester);
    }

    next();
  },
};
</script>

<style lang="scss">
footer {
  margin: 0px !important;
}
</style>
