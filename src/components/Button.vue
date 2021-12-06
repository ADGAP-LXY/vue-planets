<template>
  <button>
    <svg viewBox="0 0 50 50" class="IconPrev">
      <path
        fill-rule="evenodd"
        clip-rule="evenodd"
        d="M29 13.686l1.414 1.415-9.9 9.899 9.9 9.9L29 36.313 17.686 25 29 13.686z"
        fill="currentColor"
        aria-hidden="true"
        focusable="false"
      ></path>
    </svg>
  </button>
</template>

<script>
export default {
  name: "Button",
};
</script>

<style scoped>
button {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50px;
  height: 50px;
  background-color: #e73b54;
  border: none;
}
button:hover {
  background: rgb(29, 177, 9);
}
</style>