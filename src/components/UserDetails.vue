<template>
  <li
    text="white"
    p="2"
    class="flex gap-4 items-start border-gray-700 hover:border-violet-700 border-solid border-1 rounded-md"
  >
    <div>
      <img w="32" :src="u.avatar_url" :alt="u.login" />
    </div>
    <div class="flex gap-5 justify-between">
      <ul class="flex gap-5 flex-col sm:flex-row">
        <h4>
          {{ u.login }}
        </h4>

        <li>
          <a :href="u.html_url" class="text-blue-400" target="_blank"
            >Profile</a
          >
        </li>
      </ul>
    </div>
  </li>
</template>

<script setup>
defineProps({
  u: {
    type: Object,
    required: true,
  },
});
</script>
