<template>
  <footer class="mt-20 bg-gray-200 dark:bg-gray-800 relative">
    <div>
      <ContactShorcut />
    </div>
    <FooterCurve />
    <div class="container pt-144 md:pt-80 pb-20">
      <div class="mt-6 text-center sm:text-left flex justify-center">
        <ul class="space-y-10 sm:space-y-0 sm:flex sm:space-x-20 md:space-x-16">
          <li>
            <ul class="mt-4 space-y-3 text-sm">
              <li>
                <p class="text-sm font-extrabold opacity-80">
                  {{ $t("navigation.linksText") }}
                </p>
              </li>
              <AppFooterLink :footerLink="mobileLinks[0]" />
              <AppFooterLink :footerLink="mobileLinks[1]" />
              <AppFooterLink :footerLink="mobileLinks[2]" />
            </ul>
          </li>

          <li>
            <ul class="mt-4 space-y-3 text-sm">
              <li>
                <p class="text-sm font-extrabold opacity-80">
                  {{ $t("footer.comunity.title") }}
                </p>
              </li>

              <li>
                <AppFooterSocialItem
                  :footerSocialItem="$t('footer.comunity.telegramLink')"
                >
                  <Telegram
                    :title="$t('icons.Telegram.title')"
                    class="h-4 w-4"
                  />
                </AppFooterSocialItem>
              </li>

              <li>
                <AppFooterSocialItem
                  :footerSocialItem="$t('footer.comunity.facebookLink')"
                >
                  <Facebook
                    :title="$t('icons.Facebook.title')"
                    class="h-4 w-4"
                  />
                </AppFooterSocialItem>
              </li>

              <li>
                <AppFooterSocialItem
                  :footerSocialItem="$t('footer.comunity.twitterLink')"
                >
                  <Twitter :title="$t('icons.Twitter.title')" class="h-4 w-4" />
                </AppFooterSocialItem>
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <div class="mt-12 text-center">
        <p class="text-sm opacity-80">© 2021 Railab</p>
      </div>
    </div>
  </footer>
</template>

<script>
import AppFooterLink from "@/components/app/AppFooterLink.vue";
import AppFooterSocialItem from "@/components/app/AppFooterSocialItem.vue";
import Telegram from "@/components/icons/Telegram.vue";
import Facebook from "@/components/icons/Facebook.vue";
import Twitter from "@/components/icons/Twitter.vue";
import FooterCurve from "@/components/illustations/FooterCurve.vue";
import { navigationLinks } from "@/data/data.js";
export default {
  components: {
    AppFooterLink,
    AppFooterSocialItem,
    Telegram,
    Facebook,
    Twitter,
    FooterCurve,
    ContactShorcut: () => import("@/components/contact/ContactShorcut.vue"),
  },
  data() {
    return {
      mobileLinks: navigationLinks,
    };
  },
};
</script>

<style>
</style>