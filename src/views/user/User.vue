<template>
  <div class="user">
    <HeaderNav :navOptions="navOptions"
               key="HeaderNav"></HeaderNav>
    <section class="user-action">
      <transition>
        <UserHeader
          v-show="showHeader" 
          @show-login-event="handleShowLogin"
          @show-reg-event="handleShowReg">
        </UserHeader>
      </transition>
      <transition>
        <UserLogin v-show="showLogin"
          @show-reg-event="handleShowReg"></UserLogin>
      </transition>
      <transition>
        <UserReg v-show="showReg"></UserReg>
      </transition>
    </section>
    <transition>
      <router-view></router-view>
    </transition>
  </div>
</template>

<script>
import HeaderNav from "common/HeaderNav";
import UserHeader from "./components/UserHeader";
import UserLogin from "./components/UserLogin";
import UserReg from "./components/UserReg";
import { mapState, mapGetters } from "vuex";

export default {
  name: "User",
  data() {
    return {
      showHeader: true,
      showLogin: false,
      showReg: false,
      showCode: false
    }
  },
  methods: {
    handleShowLogin() {
      this.showHeader = this.showReg = false;
      this.showLogin = true;
    },
    handleShowReg() {
      this.showHeader = this.showLogin = false;
      this.showReg = true;
    }
  },
  computed: {
    ...mapState(['user']),
    ...mapGetters(['isLogin']),
    navOptions() {
      let op = { title: '' };
      this.showHeader && (op.title = 'USER');
      this.showLogin && (op.title = 'LOGIN');
      this.showReg && (op.title = 'REG');
      op.showMenu = this.isLogin;

      return op;
    }
  },
  components: {
    HeaderNav,
    UserHeader,
    UserLogin,
    UserReg
  }
}
</script>

<style lang="less">
.user {
  height: 100%;
  overflow: hidden;
  background: rgba(0, 0, 0, .02);
  position: relative;

  .user-action {
    margin-top: .1rem;
    position: absolute;
    top: .45rem;
    left: 0; right: 0; bottom: 0;
  }
}

.v-enter, .v-leave-to {
  transform: translateX(100%);
}

.v-enter-active, .v-leave-active {
  transition: .3s ease;
}
</style>

