<template>
  <div>
    <section>
      <h1>Notes</h1>
      <hr/>
      <br/>

      <form @submit.prevent="updateNote">
        <div class="mb-3">
          <textarea
              v-model="form.content"
              class="form-control"
          ></textarea>
        </div>
        <button type="submit" class="btn btn-primary">Update</button>
      </form>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "EventNotes",
  props: ['event_id'],
  data() {
    return {
      note: {
        content: '',
      },
      form: {
        content: '',
      },
    };
  },
  methods: {
    async getEventNotes(event_id) {
      await axios
          .get("/" + event_id + "/notes")
          .then((res) => {
            this.note = res.data;
            this.form.content = this.note.content;
          })
          .catch((error) => {
            console.error(error);
          });
    },
    async _updateNote(note) {
      await axios
          .put("/notes", note)
          .then((res) => {
            this.note = res.data;
          })
          .catch((error) => {
            console.error(error);
          });
    },
    async updateNote() {
      try {
        let note = {
          event_id: this.event_id,
          content: this.form.content,
        };
        await this._updateNote(note);
      } catch (error) {
        console.log(error);
      }
    },
  },
  created() {
    this.getEventNotes(this.event_id);
  },
};
</script>