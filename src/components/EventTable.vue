<template>
  <section class="mb-5">
    <h2 class="text-center mb-4">Available Events</h2>
    
    <div class="row mb-3">
      <div class="col-md-3 mb-2">
        <input type="text" class="form-control" v-model="eventIdFilter" placeholder="Filter by Event ID">
      </div>
      <div class="col-md-3 mb-2">
        <input type="text" class="form-control" v-model="eventNameFilter" placeholder="Filter by Event Name">
      </div>
      <div class="col-md-3 mb-2">
        <input type="number" class="form-control" v-model="durationFilter" placeholder="Filter by Duration">
      </div>
      <div class="col-md-3 mb-2">
        <div class="btn-group" role="group">
          <button type="button" class="btn btn-outline-primary" 
                  v-for="category in categories" 
                  :key="category" 
                  @click="categoryFilter = category"
                  :class="{ 'active': categoryFilter === category }">
            {{ category }}
          </button>
          <button type="button" class="btn btn-outline-primary" 
                  @click="categoryFilter = 'All'"
                  :class="{ 'active': categoryFilter === 'All' }">
            All
          </button>
        </div>
      </div>
    </div>
    
    <div class="table-responsive">
      <table class="table table-striped table-hover">
        <thead class="table-dark">
          <tr>
            <th>Event ID</th>
            <th>Event Name</th>
            <th>Category</th>
            <th>Duration (Hours)</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="event in filteredEvents" :key="event.id">
            <td>{{ event.id }}</td>
            <td>{{ event.name }}</td>
            <td>{{ event.category }}</td>
            <td>{{ event.duration }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    events: {
      type: Array,
      required: true
    },
    categories: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      eventIdFilter: '',
      eventNameFilter: '',
      durationFilter: null,
      categoryFilter: 'All'
    }
  },
  computed: {
    filteredEvents() {
      return this.events.filter(event => {
        const searchId = this.eventIdFilter.toLowerCase().trim();
        const searchName = this.eventNameFilter.toLowerCase().trim();
        const searchDuration = Number(this.durationFilter);
        
        const eventId = event.id.toString().toLowerCase();
        const eventName = event.name.toLowerCase();
        
        return (
          eventId.includes(searchId) &&
          eventName.includes(searchName) &&
          (this.durationFilter ? event.duration === searchDuration : true) &&
          (this.categoryFilter === 'All' || event.category === this.categoryFilter)
        );
      });
    }
  }
}
</script>