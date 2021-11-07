<template>
    <div class="container">
      <div class="row">
        <div class="col-md-4">
          <Range v-model="value" />
        </div>
        <div class="col-md-4">
          <ListUsers />
        </div>
      </div>
    </div>
</template>

<script>
import Range from "@/components/Range.vue";
import ListUsers from "@/components/ListUsers.vue";

export default {
  name: 'Home',
  data() {
    return {
      value: 50
    }
  },
  components: {
    Range,
    ListUsers
  }
}
</script>
