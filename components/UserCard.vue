    <template>
      <nuxt-link :to="'/users/' + event.id">
        <div class="-shadow">
          <span class="eyebrow">
            @{{ event.time }} on {{ parsedDate }}
          </span>
          <h4 class="title">
            {{ event.name }}
          </h4>
         /*  <span>{{ event.attendees.length }} attending</span> */
        </div>
      </nuxt-link>
    </template>
    <script>
    export default {
      name: 'UserCard',
      props: {
        // eslint-disable-next-line vue/require-default-prop
        event: Object
      },
      computed: {
        parsedDate() {
          const eventDate = new Date(this.event.date)
          return eventDate.toDateString()
        }
      }
    }
    </script>

