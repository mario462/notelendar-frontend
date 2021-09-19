<template>
  <div>
    <section>
      <h1>Events</h1>
      <hr />
      <br />

      <div v-if="events.length">
        <div v-for="event in events" :key="event.id" class="events">
          <div class="card">
            <div class="card-body">
              <ul>
                <li><strong>Summary:</strong> {{ event.summary }}</li>
                <li><strong>Status:</strong> {{ event.status }}</li>
                <li><strong>Start:</strong> {{ event.start.dateTime }}</li>
                <li><strong>End:</strong> {{ event.end.dateTime }}</li>
                <div v-for="attendee in event.attendees" :key="attendee.email">
                  <li>
                    <strong>Attendee:</strong> {{ attendee.email }}
                    <a v-if="attendee.self"><strong>(You)</strong></a>
                    <a v-if="attendee.organizer"
                      ><strong>(Organizer)</strong></a
                    >
                  </li>
                </div>
                <li>
                  <a :href="event.htmlLink" target="_blank"
                    >Open event in Google Calendar</a
                  >
                </li>
                <li>
                  <router-link
                    :to="{ name: 'Event Notes', params: { id: event.id } }"
                    >View event notes</router-link
                  >
                </li>
                <div v-if="event.hangoutLink">
                  <li>
                    <a :href="event.hangoutLink" target="_blank">Join call</a>
                  </li>
                </div>
              </ul>
            </div>
          </div>
          <br />
        </div>
      </div>

      <div v-else>
        <p>No upcoming events today.</p>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  name: "DailyEvents",
  data() {
    return {
      events: [],
    };
  },
  computed: {
    today: function() {
      return moment().format("YYYY-MM-DD");
    }
  },
  methods: {
    getEvents() {
      axios
        .get("/", { params: { day: this.today } })
        .then((res) => {
          this.events = res.data;
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
  created() {
    this.getEvents();
  },
};
</script>