<template>
    <div class="root-view" ref="rootView">
        <transition :name="transitionName">
            <keep-alive>
                <router-view class="child-view" v-if="$route.meta.keepAlive"></router-view>
            </keep-alive>
        </transition>
        <transition :name="transitionName">
            <router-view class="child-view" :class="transitionName" v-if="!$route.meta.keepAlive"></router-view>
        </transition>
    </div>
</template>

<script type="text/ecmascript-6">
export default {
    data() {
        return {
            transitionName: 'router-slide' // 可选值：router-fade, toggle
        };
    },
    beforeRouteUpdate(to, from, next) {
        // 如果isBack为true时，证明是用户点击了回退，执行slide-right动画
        let isBack = this.$router.isBack;
        console.log(isBack);
        if (isBack) {
            this.transitionName = 'slide-right';
        } else {
            this.transitionName = 'slide-left';
        }
        // 做完回退动画后，要设置成前进动画，否则下次打开页面动画将还是回退
        this.$router.isBack = false;
        next();
    },
    components: {

    }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import './style/index.styl';

.child-view,
.root-view {
    width: 100%;
    height: 100%;
    max-width: 100%;
    z-index: 1;
}
.child-view.black-bg {
    background-color: #272822;
    color: azure;
}

.router-fade-enter-active,
.router-fade-leave-active {
    transition: opacity 0.3s;
}
.router-fade-enter,
.router-fade-leave-active {
    opacity: 0;
}

.router-slide-enter-active,
.router-slide-leave-active {
    transition: all 0.4s;
}

.router-slide-enter,
.router-slide-leave-active {
    transform: translate3d(2rem, 0, 0);
    opacity: 0;
}

.toggle-enter-active,
.toggle-leave-active {
    transition: all 0.3s ease-out;
}
.toggle-enter,
.toggle-leave-active {
    transform: translateX(100%);
}

.cube-content
    position: absolute
    top: 44px
    left: 0
    right: 0
    bottom: 0
    overflow: scroll
    -webkit-overflow-scrolling: touch
    .cube-list
      padding-left: 10px
      .cube-item
        height: 40px
        line-height: 40px
        .link
          display: block
          position: relative
          width: 100%
          color: #333
          text-decoration: none
          outline: 0
          .cubeic-arrow
            position: absolute
            right: 0
            padding: 0 5px
            color: #ccc;
</style>
