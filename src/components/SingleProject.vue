<template>
  <div class="projects" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
      <div class="icons">
        <span @click="toggleComplete" class="material-icons tick"> done </span>
        <span @click="deleteProject" class="material-icons"> delete </span>
      </div>
    </div>
    <div v-if="showDetails" class="details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetails: false,
      uri: "http://localhost:3000/projects/" + this.project.id,
    };
  },
  methods: {
    deleteProject() {
      fetch(this.uri, { method: "DELETE" })
        .then(() => this.$emit("delete", this.project.id))
        .catch((err) => console.log(err));
    },
    toggleComplete() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ complete: !this.project.complete }),
      })
        .then(() => {
          this.$emit("complete", this.project.id);
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style>
.projects {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  border-left: 4px solid crimson;
}

h3 {
  cursor: pointer;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.material-icons {
  font-size: 24px;
  margin-left: 10px;
  cursor: pointer;
  transition: 0.5s;
  color: #bbb;
}
.material-icons:hover {
  color: green;
}
.projects.complete {
  border-left: 3px solid greenyellow;
}
.projects.complete .tick {
  color: greenyellow;
}
</style>