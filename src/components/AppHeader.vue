<template>
  <div class="container mt-4">
    <h1 class="text-center mb-4">Event Management System</h1>
    
    <div class="text-center mb-5">
      <img src="../assets/event-management.jpg" alt="Event Management" class="img-fluid rounded" style="max-height: 300px;">
    </div>
    
    <section class="mb-5">
      <h2 class="text-center mb-4">Why Choose Us?</h2>
      <div class="row">
        <div class="col-12 col-md-6 col-lg-4 mb-4" v-for="(item, index) in features" :key="index">
          <div class="card h-100">
            <div class="card-body">
              <h5 class="card-title">{{ item.title }}</h5>
              <p class="card-text">{{ item.description }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  props: {
    features: Array
  }
};
</script>

<style scoped>
.card {
  transition: transform 0.2s;
}
.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
}
</style>
