<template>
  <TheHeader />
  <main
    :h="!inputTouched ? 'full' : 'auto'"
    w="full max-4xl"
    m="x-auto"
    class="flex gap-5 flex-col justify-center"
    p="2"
  >
    <!-- Search -->
    <div w="md:2xl" m="x-auto" class="flex flex-col gap-2">
      <label text="[#c9d1d9] xl" for="search-user" p="2"
        >Search for a GitHub user</label
      >
      <form
        @submit.prevent="handleSearch"
        w="full"
        class="flex items-center gap-2"
      >
        <input
          w="full"
          m="x-auto"
          bg="inherit"
          text="[#c9d1d9] xl"
          type="search"
          name="search-user"
          id="search-user"
          placeholder="Search"
          class="border-[#30363d] border-1 border-solid"
          p="2"
          @focus="($event) => handleTouch($event)"
          @blur="($event) => handleTouch($event)"
          v-model="searchText"
          autocomplete="off"
        />
        <button
          :disabled="!searchText.length ? true : false"
          bg="[#21262d]"
          w="24"
          p="2"
          text="[#c9d1d9] xl"
          class="rounded-md border-1 border-[#30363d] border-solid hover:border-[#8b949e] hover:transition-all"
        >
          Go
        </button>
      </form>
    </div>
    <!-- Results -->
    <ul
      v-if="users.list.length"
      text="white"
      w="max-4xl"
      class="flex gap-3 flex-col"
    >
      <div class="flex justify-between items-center">
        <p>Found total of {{ users.total }}</p>
        <button @click="users.list = []">Clear</button>
      </div>
      <UserDetails v-for="u in users.list" :key="u.id" :u="u" />
    </ul>
    <nav
      v-if="users.list.length"
      m="x-auto"
      w="max-xl"
      class="flex gap-12 justify-between items-center"
      text="white"
      p="4 lg:10"
    >
      <button
        @click="handlePagination('prev')"
        w="28"
        class="bg-[#21262d] rounded-md border-1 border-[#30363d] border-solid hover:border-[#8b949e] hover:transition-all p-2"
      >
        Previous
      </button>
      <button
        @click="handlePagination('next')"
        w="28"
        class="bg-[#21262d] rounded-md border-1 border-[#30363d] border-solid hover:border-[#8b949e] hover:transition-all p-2"
      >
        Next
      </button>
    </nav>
  </main>
</template>

<script setup>
import { ref } from "vue";
import TheHeader from "./components/TheHeader.vue";
import UserDetails from "./components/UserDetails.vue";

const inputTouched = ref(false);

const searchText = ref("");
const users = ref({
  total: 0,
  list: [],
});

let currentPage = 1;
const handlePagination = (direction) => {
  if (direction === "next") {
    currentPage++;
    handleSearch(currentPage);
  }
  if (direction === "prev") {
    currentPage--;
    handleSearch(currentPage);
  }
};

const handleTouch = (e) => {
  if (e.type === "focus") {
    inputTouched.value = true;
  }
  if (
    !searchText.value.trim() &&
    e.type === "blur" &&
    !users.value.list.length
  ) {
    inputTouched.value = false;
  }
};

const handleSearch = async () => {
  let result;
  try {
    const response = await fetch(
      `https://api.github.com/search/users?q=${searchText.value}&page=${currentPage}`
    );
    result = await response.json();
  } catch (error) {
    console.log(error);
  }

  users.value.list = result.items;
  users.value.total = result.total_count;
};
</script>

<style>
*,
*::after,
*::before {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html,
body {
  height: 100%;
  background-color: #0d1117;
  overflow: hidden;
}

#app {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow-y: auto;
}
</style>
