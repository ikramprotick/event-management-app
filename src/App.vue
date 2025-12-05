<template>
  <div class="container mt-4">
    <app-header :features="features" />
    <event-table :events="events" :categories="categories" />
    <registration-form 
      :events="events" 
      :categories="categories"
      @form-submitted="handleFormSubmission"
    />
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue';
import EventTable from './components/EventTable.vue';
import RegistrationForm from './components/RegistrationForm.vue';

export default {
  components: {
    AppHeader,
    EventTable,
    RegistrationForm
  },
  data() {
    return {
      features: [
        { title: "Wide Variety", description: "Choose from hundreds of events across multiple categories." },
        { title: "Expert Speakers", description: "Learn from industry leaders and subject matter experts." },
        { title: "Networking Opportunities", description: "Connect with like-minded professionals and peers." },
        { title: "Flexible Scheduling", description: "Events available at various times to suit your schedule." },
        { title: "Affordable Pricing", description: "Quality events at competitive prices." },
        { title: "Certification", description: "Earn certificates to enhance your professional profile." }
      ],
      events: [],
      categories: ['Technology', 'Business', 'Marketing', 'Finance']
    }
  },
  created() {
    this.loadEvents();
  },
  methods: {
    async loadEvents() {
      try {
        const response = await fetch('events.txt');
        const data = await response.text();
        
        this.events = data
          .trim()
          .slice(1, -1)
          .split('},')
          .map(item => {
            const objString = item.trim().replace(/{|}/g, '');
            const entries = objString.split(',')
              .map(entry => {
                const [key, value] = entry.split(':').map(s => s.trim().replace(/'/g, ''));
                return { key, value };
              });
            
            return {
              id: entries.find(e => e.key === 'eventid').value,
              name: entries.find(e => e.key === 'eventname').value,
              category: entries.find(e => e.key === 'category').value,
              duration: parseInt(entries.find(e => e.key === 'durationhour').value)
            };
          });
      } catch (error) {
        console.error('Error loading events:', error);
        this.events = [
          {id: 'EVT10001', name: 'Tech Innovations Conference', category: 'Technology', duration: 8},
          // ... rest of the fallback events ...
        ];
      }
    },
    handleFormSubmission(formData) {
      console.log('Form submitted:', formData);
      // Handle form submission logic here
    }
  }
}
</script>