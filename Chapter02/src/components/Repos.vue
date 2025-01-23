<template>
  <div class="repos-container">
    <h1>Repos</h1>
    <div class="grid">
      <!-- Use grid layout for all items -->
      <div
        v-for="r in repos"
        :key="r.id"
        class="grid-item"
        @mouseover="hover = r.id"
        @mouseleave="hover = null"
        :class="{ 'hovered': hover === r.id }"
      >
      <h2><a :href="r.html_url" target="_blank">{{ r.owner.login }}/{{ r.name }}</a></h2><Issues :owner="r.owner.login" :repo="r.name" />
      </div>
    </div>
  </div>
</template>

<script>
import Issues from "./repo/Issues.vue";
import { octokitMixin } from "../mixins/octokitMixin";

export default {
  name: "Repos",
  components: {
    Issues,
  },
  data() {
    return {
      repos: [],
      hover: null, // Track which card is being hovered
    };
  },
  mixins: [octokitMixin],
  async mounted() {
    const octokit = this.createOctokitClient();
    const { data: repos } = await octokit.request("/user/repos");
    this.repos = repos;
  },
};
</script>

<style scoped>

:root {
  --color1: #FF6B6B;
  --color2: #FFB86C;
  --color3: #9B59B6;
  --color4: #54C571;
  --color5: #5DADE2;
  --color6: #E67E22;
}
.repos-container {
  padding: 20px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 16px;
}


.grid-item:nth-child(2n) {
  background-color: var(--color2);
}

.grid-item:nth-child(3n) {
  background-color: var(--color3);
}

.grid-item:nth-child(4n) {
  background-color: var(--color4);
}

.grid-item:nth-child(5n) {
  background-color: var(--color5);
}

.grid-item:nth-child(6n) {
  background-color: var(--color6);
}


.grid-item a {
  color: #0077b6;
  text-decoration: none;
}

.grid-item a:hover {
  color: #005b8f;
}
.grid-item h2 {
  margin-top: 0;
  font-size: 1.2rem;
  word-break: break-all;
}
.grid-item:hover {
  transform: translateY(-10px) scale(1.05); /* Lift and scale effect */
  box-shadow: 0 6px 12px rgba(0,0,0,0.2); /* Enhanced shadow on hover */
}

.hovered {
  transform: translateY(-10px) scale(1.05); /* Same as hover but reactive */
}

/* Optional responsive styles */
@media (max-width: 768px) {
  .grid {
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 12px;
  }
}
</style>
