<template>
  <header class="header">
    <div class="header__sticky">
      <div class="header__nav">
        <a
          v-if="!isHaveSurah"
          class="header__hamburger"
          href="javascript:void(0)"
          title="Open Sidebar"
          @click="toggleSidebar">
          <svg
            viewBox="0 0 512 512"
            width="30px"
            height="30px"
            fill="#fff">
            <path d="M64 384h384v-42.666H64V384zm0-106.666h384v-42.667H64v42.667zM64 128v42.665h384V128H64z"/>
          </svg>
        </a>
        <nuxt-link
          v-else
          to="/?source=surah"
          class="header__hamburger">
          <svg
            viewBox="0 0 512 512"
            width="30px"
            height="30px"
            fill="#fff">
            <path d="M427 234.625H167.296l119.702-119.702L256 85 85 256l171 171 29.922-29.924-118.626-119.701H427v-42.75z"/>
          </svg>
        </nuxt-link>
      </div>
      <div
        class="header__content">
        <nuxt-link
          v-if="!isHaveSurah"
          to="/"
          class="header__title">
          <h1>{{ appTitle }}</h1>
        </nuxt-link>
        <h1 v-else >{{ surah }}</h1>
      </div>
      <div
        v-if="isHaveSurah"
        class="header__nav pointer"
        @click="toggleMenuRight">
        <svg
          viewBox="0 0 512 512"
          width="30px"
          height="30px"
          fill="#fff">
          <path d="M296 136c0-22.002-17.998-40-40-40s-40 17.998-40 40 17.998 40 40 40 40-17.998 40-40zm0 240c0-22.002-17.998-40-40-40s-40 17.998-40 40 17.998 40 40 40 40-17.998 40-40zm0-120c0-22.002-17.998-40-40-40s-40 17.998-40 40 17.998 40 40 40 40-17.998 40-40z"/>
        </svg>
      </div>
      <div
        v-else
        class="header__nav">
        &nbsp;
      </div>
    </div>
    <nav
      v-show="isShowMenu"
      class="menu_right" >
      <ul>
        <li>
          <div
            class="menu_link"
            @click="navigateTo('https://github.com/mazipan/quran-offline')">
            Tentang
          </div>
        </li>
        <li>
          <div
            class="menu_link"
            @click="navigateTo('https://github.com/mazipan/quran-offline')">
            Kode Sumber
          </div>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script>
import { EventBus } from '../eventbus/index'
import { AppConstant } from '../constant/index'
import { __isNotEmptyString } from '../utils/index'

export default {
  name: 'BaseHeader',
  props: {
    surah: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      isShowMenu: false,
      appTitle: AppConstant.TITLE
    };
  },
  computed: {
    isHaveSurah() {
      return __isNotEmptyString(this.surah)
    }
  },
  methods: {
    navigateTo: function (link) {
      this.toggleMenuRight();
      if (link.indexOf('http') >= 0) {
        window.location.href = link
      } else {
        this.$router.push(link)
      }
    },
    toggleMenuRight() {
      this.isShowMenu = !this.isShowMenu
      if (this.isShowMenu) {
        setTimeout(() => {
          this.isShowMenu = false
        }, 2000)
      }
    },
    toggleSidebar() {
      EventBus.$emit('toggleSidebar');
    }
  }
};
</script>

<style lang="scss" scoped>
@import '../assets/header.scss';
.menu_right{
  position: absolute;
  top: 50px;
  right: 30px;
  z-index: 99;

  ul {
    margin: 0;
    padding: 0;
    list-style: none;

    li{
      .menu_link{
          display: block;
          background-color: #000000;
          color: $theme;
          text-decoration: none;
          padding: 15px 25px;
          font-size: 16px;
          display: flex;
          align-items: center;
        }
    }
  }
}
</style>
