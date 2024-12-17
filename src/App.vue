<template>
  <div class="container">
    <!-- Full-screen hero -->
    <section class="hero">
      <h1 class="hero-text">HELLO WORLD</h1>
    </section>

    <!-- Timeline section -->
    <section class="timeline">
      <div class="timeline-container">
        <!-- Year markers -->
        <div 
          class="year-marker" 
          v-for="year in years" 
          :key="year"
          :style="yearPositionStyle(year)"
        >
          <div class="year-box">{{ year }}</div>
        </div>

        <!-- Event markers -->
        <div
          class="event-item"
          v-for="(event, index) in events"
          :key="index"
          :style="eventPositionStyle(event.date)"
        >
          <div class="event-box">
            <div class="event-date">{{ formatDate(event.date) }}</div>
            <div class="event-description">{{ event.description }}</div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

// Helper function to parse dates in the format YYYY-DD-MM (as given).
function parseCustomDate(str: string): Date {
  // Expected format: YYYY-DD-MM
  const parts = str.split('-')
  const year = parseInt(parts[0], 10)
  const day = parseInt(parts[1], 10)
  const month = parseInt(parts[2], 10)
  // JavaScript Date constructor: month is 0-based
  return new Date(year, month - 1, day)
}

export default defineComponent({
  name: 'App',
  setup() {
    const years = [2018, 2019, 2020, 2021, 2022, 2023, 2024]

    // Events with given dates. Add descriptions as needed.
    const events = [
      { date: parseCustomDate('2020-30-06'), description: 'An event in mid 2020.' },
      { date: parseCustomDate('2018-22-04'), description: 'An event in early 2018.' },
      { date: parseCustomDate('2023-15-11'), description: 'An event in late 2023.' },
    ]

    // Timeline start/end
    const startDate = new Date(2018, 0, 1) // Start of 2018
    const endDate = new Date(2025, 0, 1)   // Start of 2025
    const totalDuration = endDate.getTime() - startDate.getTime()

    // Vertical space representing full timeline (in px)
    const timelineHeight = 2100 // Adjust as needed

    // Given a year, returns a style that places it on the timeline
    // We'll position years linearly as well.
    function yearPositionStyle(year: number) {
      const yearStart = new Date(year, 0, 1)
      const fraction = (yearStart.getTime() - startDate.getTime()) / totalDuration
      const top = fraction * timelineHeight
      return {
        position: 'absolute',
        top: `${top}px`,
        left: '50%',
        transform: 'translateX(-50%)'
      }
    }

    // Given a date, returns a style that places the event on the timeline
    function eventPositionStyle(date: Date) {
      const fraction = (date.getTime() - startDate.getTime()) / totalDuration
      const top = fraction * timelineHeight
      return {
        position: 'absolute',
        top: `${top}px`,
        left: '50%',
        transform: 'translateX(-50%)'
      }
    }

    function formatDate(date: Date) {
      // Format as dd-mm-yyyy for display
      const d = String(date.getDate()).padStart(2, '0')
      const m = String(date.getMonth() + 1).padStart(2, '0')
      const y = date.getFullYear()
      return `${d}-${m}-${y}`
    }

    return { years, events, yearPositionStyle, eventPositionStyle, formatDate }
  }
})
</script>

<style scoped>
.container {
  width: 100%;
  overflow-x: hidden;
}

/* Hero section */
.hero {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background: #333;
  color: #fff;
  margin: 0;
}

.hero-text {
  font-size: 6rem;
  text-transform: uppercase;
  margin: 0;
}

/* Timeline section */
.timeline {
  background: #111;
  /* Set height large enough to see events */
  height: 2200px; 
  display: flex;
  align-items: flex-start;
  justify-content: center;
  position: relative;
  margin: 0;
}

.timeline-container {
  position: relative;
  width: 2px;
  background: #666;
  margin-top: 100px;
}

/* Year markers */
.year-box {
  background: #222;
  padding: 10px 20px;
  border-radius: 8px;
  font-size: 1.2rem;
  font-weight: bold;
  border: 1px solid #555;
  color: #fff;
}

/* Event items */
.event-box {
  background: #444;
  padding: 10px 15px;
  border-radius: 8px;
  border: 1px solid #555;
  color: #fff;
  text-align: center;
  max-width: 200px;
}

.event-date {
  font-weight: bold;
  margin-bottom: 5px;
}

.event-description {
  font-size: 0.9rem;
  opacity: 0.9;
}
</style>
