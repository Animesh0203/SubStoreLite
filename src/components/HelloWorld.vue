<template>
  <div class="container mx-auto px-4 py-6 max-w-md">
    <h1 class="text-2xl font-bold mb-6 text-center">Add A Sub!</h1>

    <!-- Text Input -->
    <div class="mb-4">
      <label class="block text-sm font-medium mb-2">Name Your Sub:</label>
      <input v-model="newPost.text" placeholder="Enter some text" class="border rounded w-full p-2" />
    </div>

    <!-- Toggle For Veg or Non-Veg -->
    <label class="themeSwitcherTwo relative inline-flex cursor-pointer select-none items-center">
      <input type="checkbox" class="sr-only" v-model="isChecked" />
      <span class="label flex items-center text-sm font-medium text-dark dark:text-green-400">
        Veg
      </span>
      <span
        class="slider mx-4 flex h-8  items-center rounded-full bg-[#CCCCCE] p-1 duration-200"
      >
        <span :class="{'dot': true, 'bg-green-500': !isChecked, 'bg-red-500': isChecked}" class="h-6 w-6 rounded-full duration-200"></span>
      </span>
      <span class="label flex items-center text-sm font-medium text-dark dark:text-red-400">
        Non-Veg
      </span>
    </label>
    <br>
    <br>

    <!-- Radio Buttons -->
    <div class="mb-4">
      <label class="block text-sm font-medium mb-2">Select your Bread:</label>
      <div v-for="bread in breads" :key="bread" class="space-y-2">
        <label class="flex items-center">
          <input type="radio" :value="bread" v-model="newPost.bread" class="mr-2" />{{bread}}
        </label>
      </div>
    </div>

    <!-- Patty Radio Buttons -->
    <div class="mb-4">
      <label class="block text-sm font-medium mb-2">Select a Patty:</label>
      <div v-if="isChecked">
      <div v-for="meatPatty in proteinsNonVegetarian" :key="meatPatty"  class="space-y-2">
        <!-- Non-Vegetarian Patties -->
        <label class="flex items-center" >
          <input type="radio" :value="meatPatty" v-model="newPost.patty" class="mr-2" /> {{ meatPatty }}
        </label>
      </div>
    </div>
    <div v-else>
      <div v-for="vegPatty in proteinsVegetarian" :key="vegPatty" class="space-y-2">
        <!-- Vegetarian Patties -->
        <label class="flex items-center" >
          <input type="radio" :value="vegPatty" v-model="newPost.patty" class="mr-2" /> {{ vegPatty }}
        </label>
      </div>
    </div>
    </div>

    <!-- Checkboxes -->
    <div class="mb-4">
      <label class="block text-sm font-medium mb-2">Select your veggies:</label>
      <div v-for="veg in vegetables" :key="veg" class="space-y-2">
        <label class="flex items-center">
          <input type="checkbox" value="Checkbox 1" v-model="newPost.checkboxes" class="mr-2" /> {{ veg }}
        </label>
      </div>
    </div>

        <!-- Checkboxes -->
        <div class="mb-4">
      <label class="block text-sm font-medium mb-2">Select your sauces:</label>
      <div v-for="sauce in sauces" :key="sauce" class="space-y-2">
        <label class="flex items-center">
          <input type="checkbox" value="Checkbox 1" v-model="newPost.checkboxes" class="mr-2" /> {{ sauce }}
        </label>
      </div>
    </div>

        <!-- Checkboxes -->
        <div class="mb-4">
      <label class="block text-sm font-medium mb-2">Select your seasonings:</label>
      <div v-for="sea in seasonings" :key="sea" class="space-y-2">
        <label class="flex items-center">
          <input type="checkbox" value="Checkbox 1" v-model="newPost.checkboxes" class="mr-2" /> {{ sea }}
        </label>
      </div>
    </div>

    <!-- Add Post Button -->
    <button @click="addPost" class="bg-green-500 text-white p-3 rounded w-full hover:bg-green-600 transition">
      Add Post
    </button>

    <!-- Display Saved Posts -->
    <div v-if="posts.length" class="mt-6">
      <h2 class="text-xl font-semibold mb-4">Saved Posts</h2>
      <div v-for="(post, index) in posts" :key="index" class="border-b py-4">
        <p class="text-sm"><strong>Text:</strong> {{ post.text }}</p>
        <p class="text-sm"><strong>Selected Bread:</strong> {{ post.bread }}</p>
        <p class="text-sm"><strong>Selected Patty:</strong> {{ post.patty }}</p>
        <p class="text-sm"><strong>Selected Checkboxes:</strong> {{ post.checkboxes.join(', ') }}</p>
        <button @click="deletePost(index)" class="text-red-500 mt-2 underline">Delete</button>
      </div>
    </div>
    <p v-else class="mt-6 text-gray-500 text-center">No posts available.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      breads: [
        "Multigrain bread",
        "Roasted Garlic bread",
        "Honey Oat bread",
        "Italian bread",
        "Flatbread"
      ],
      vegetables: [
        "Lettuce",
        "Tomato",
        "Cucumber",
        "Onion",
        "Capsicum (Bell peppers)",
        "Pickles",
        "Jalapeños",
        "Olives",
        "Carrot"
      ],
      proteinsNonVegetarian: [
        "Chicken Tikka",
        "Chicken Seekh Kebab",
        "Chicken Teriyaki",
        "Roasted Chicken Breast",
        "Tuna",
        "Turkey Breast",
        "Ham",
        "Chicken Slice",
        "Chicken Kofta"
      ],
      proteinsVegetarian: [
        "Aloo Patty (Potato Patty)",
        "Paneer Tikka",
        "Veg Shammi",
        "Hara Bhara Kebab",
        "Veggie Delight"
      ],
      cheeses: [
        "Cheddar Cheese",
        "Mozzarella Cheese"
      ],
      sauces: [
        "Mayonnaise",
        "Honey Mustard",
        "Mint Mayonnaise",
        "Chipotle Southwest",
        "Sweet Onion",
        "Red Chili",
        "Barbecue",
        "Tandoori Mayo",
        "Hot Sauce"
      ],
      seasonings: [
        "Salt",
        "Pepper",
        "Oregano",
        "Chili Flakes"
      ],
      newPost: {
        text: '',
        bread: '', // Changed from radio to bread
        patty: '', // Added patty selection
        checkboxes: [] // Checkbox selection
      },
      posts: [],
      isChecked: false // Toggle state for Veg/Non-Veg
    };
  },
  created() {
    this.loadPosts();
  },
  methods: {
    addPost() {
      if (this.newPost.text.trim()) {
        this.posts.push({
          text: this.newPost.text,
          bread: this.newPost.bread,
          patty: this.newPost.patty,
          checkboxes: [...this.newPost.checkboxes] // Properly handle array
        });
        this.savePosts();
        this.resetForm();
      }
    },
    handleCheckboxChange() {
      this.isChecked = !this.isChecked; // Update toggle state
    },
    savePosts() {
      localStorage.setItem('posts', JSON.stringify(this.posts));
    },
    loadPosts() {
      const storedPosts = localStorage.getItem('posts');
      if (storedPosts) {
        this.posts = JSON.parse(storedPosts);
      }
    },
    deletePost(index) {
      this.posts.splice(index, 1);
      this.savePosts();
    },
    resetForm() {
      this.newPost = {
        text: '',
        bread: '',
        patty: '',
        checkboxes: []
      };
    }
  }
};
</script>
