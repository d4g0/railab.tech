<template>
  <div>
    <div class="relative">
      <button
        ref="controlBtn"
        :aria-label="$t('langSwitcher.label')"
        aria-haspopup="true"
        aria-controls="lang_menu"
        class="p-2 rounded-xl border w-full h-full focus:outline-none focus-visible:ring-4 focus-visible:ring-primary-light dark:focus-visible:ring-primary-dark hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors duration-150 ease-linear"
      >
        <svg
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
          class="stroke-current w-full h-full"
        >
          <use href="/icons/lang-switch.svg#lang-switch" />
        </svg>
      </button>

      <!-- language options -->
      <transition name="fade-from-up">
        <div
          id="lang_menu"
          ref="controlMenu"
          v-show="switchIsOpen"
          class="absolute z-20 bg-white dark:bg-surface-dark border rounded-md py-2 px-4 shadow-md"
          aria-label="Language Options"
          :class="menuPositionClasses"
        >
          <ul :aria-label="$t('langSwitcher.optionsLabel')">
            <li v-for="locale in availableLocales" :key="locale.code">
              <nuxt-link
                class="hover:bg-gray-100 dark:hover:bg-gray-800 block py-1 px-2 rounded-lg text-sm focus:outline-none focus:ring-4 focus:ring-primary-light dark:focus:ring-primary-dark"
                :to="switchLocalePath(locale.code)"
                @click.native="toogleSwitch('close')"
                >{{ locale.name }}</nuxt-link
              >
            </li>
          </ul>
        </div>
      </transition>
    </div>

    <!-- overlay -->
    <transition name="fade">
      <div
        v-if="switchIsOpen"
        @click="toogleSwitch('close')"
        class=" opacity-75 fixed z-10 inset-0"
      >
      <!-- TODO set tabTrabing bg-gray-100 dark:bg-surface-dark -->
      </div>
    </transition>
  </div>
</template>

<script>
import { useEventListener, useTimeoutFn } from "@vueuse/core";
import {
  computed,
  onMounted,
  reactive,
  ref,
  toRefs,
} from "@vue/composition-api";
export default {
  props: {
    menuPosition: {
      type: String,
      default: "bottom-right",
    },
  },
  computed: {
    availableLocales() {
      return this.$i18n.locales.filter((i) => i.code !== this.$i18n.locale);
    },
    menuPositionClasses() {
      if (this.menuPosition == "bottom-right") {
        return "top-10 right-0";
      }
    },
  },

  setup() {
    const controlBtn = ref(null);
    const controlMenu = ref(null);
    const state = reactive({
      switchIsOpen: false,
    });
    var langMenuFocussables;

    onMounted(() => {
      // console.log("onMounted");
      initMenuFocusables();
    });

    /**
     * switcState: 'open' || 'close' || null -> toggle
     */
    function toogleSwitch(switchState) {
      if (process.client) {
        if (!switchState) {
          state.switchIsOpen = !state.switchIsOpen;

          if (state.switchIsOpen && langMenuFocussables.length) {
            langMenuFocussables[0].focus();
          }

          return;
        }
        switchState === "open"
          ? (state.switchIsOpen = true)
          : (state.switchIsOpen = false);

        if (langMenuFocussables.length) {
          langMenuFocussables[0].focus();
        }
      }
    }

    function handleKey(evt) {
      // tabTrabing   TODO
      // if (
      //   evt.code == "Tab" &&
      //   state.switchIsOpen &&
      //   langMenuFocussables.length
      // ) {
      //   // console.log(
      //   //   "TT focusing: " + langMenuFocussables[langMenuFocussables.length - 1]
      //   // );
      //   // backwards
      //   if (evt.shiftKey) {
      //     if (evt.target === langMenuFocussables[0]) {
      //       langMenuFocussables[langMenuFocussables.length - 1].focus();
      //     }
      //     // forward
      //     else {
      //       if (
      //         evt.target === langMenuFocussables[langMenuFocussables.length - 1]
      //       ) {
      //         langMenuFocussables[0].focus();
      //       }
      //     }
      //   }
      // }

      if (evt.code == "Escape") {
        toogleSwitch("close");
      }
    }

    function handleClick(evt) {
      if (process.client) {
        // if was in btn
        if (controlBtn.value.contains(evt?.target)) {
          toogleSwitch();

          if (state.switchIsOpen) {
            const { isActive, start, stop } = useTimeoutFn(() => {
              controlMenu.value.querySelector("a").focus();
            }, 1);
            start();
          }
        }
      }
    }

    function initMenuFocusables() {
      if (process.client) {
        langMenuFocussables = Array.from(
          controlMenu.value.querySelectorAll("a")
        );
      }
    }

    // -------
    // EVENTS
    // -------
    useEventListener("keyup", handleKey);
    useEventListener("click", handleClick);

    return {
      toogleSwitch,
      controlBtn,
      controlMenu,
      ...toRefs(state),
    };
  },
};
</script>

<style>
</style>