<template>
  <a
    target="_blank"
    rel="noopener"
    :href="contactItem.link"
    :title="contactItem.tooltip"
    class="shadow bg-gray-100 hover:bg-gray-200 dark:bg-gray-800 dark:hover:bg-gray-900 rounded-2xl py-4 px-16 flex justify-center items-center focus:outline-none focus-visible:ring focus-visible:ring-primary-light dark:focus-visible:ring-primary-dark "
  >
    <slot></slot>
    <p class="ml-2">
      {{ contactItem.body }}
    </p>
  </a>
</template>

<script>
export default {
  props: {
    contactItem: {
      type: Object,
      default: () => ({
        link: "#",
        tooltip: "#",
        body: "#",
      }),
    },
  },
};
</script>

<style>
</style>