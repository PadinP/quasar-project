<template>
  <q-page class="relative-position">
    <q-scroll-area class="absolute full-width full-height">
      <div class="q-py-lg q-px-md row items-end q-col-gutter-md">
        <div class="col">
          <q-input
            class="new-qweet"
            bottom-slots
            v-model="newQweetContent"
            placeholder="What's happenning"
            counter
            autogrow
            maxlength="280"
          >
            <template v-slot:before>
              <q-avatar size="xl">
                <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
              </q-avatar>
            </template>
          </q-input>
        </div>

        <div class="col col-shrink">
          <q-btn
            @click="addNewQweet"
            :disable="!newQweetContent"
            class="q-mb-lg"
            no-caps
            unelevated
            rounded
            color="primary"
            label="Qweet"
          />
        </div>
      </div>
      <q-separator class="divider" size="10px" color="grey-2" />

      <q-list separator>
        <transition-group
          appear
          enter-active-class="animated fadeIn"
          leave-active-class="animated fadeOut"
        >
          <q-item v-for="(qweets, index) in qweets" :key="qweets.date" class="q-py-md">
            <q-item-section avatar top>
              <q-avatar size="xl">
                <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
              </q-avatar>
            </q-item-section>

            <q-item-section>
              <q-item-label class="text-subtitle1">
                <strong>Alex Padín</strong>
                <span class="text-grey-7">
                  @alex_padin<br class="lt-md" />&bull; {{ relativeDates[index] }}
                </span>
              </q-item-label>
              <q-item-label class="qweet-content text-body1">{{ qweets.content }} </q-item-label>
              <div class="qweet-icons row justify-between q-mt-sm">
                <q-btn flat round color="grey" icon="far fa-comment" size="sm" />
                <q-btn flat round color="grey" icon="far fa-retweet" size="sm" />
                <q-btn flat round color="grey" icon="far fa-heart" size="sm" />
                <q-btn
                  @click="deleteQweet(qweets)"
                  flat
                  round
                  color="grey"
                  icon="far fa-trash"
                  size="sm"
                />
              </div>
            </q-item-section>
          </q-item>
        </transition-group>
      </q-list>
    </q-scroll-area>
  </q-page>
</template>

<script>
import { formatDistance } from 'date-fns'

export default {
  name: 'PageHome',
  data() {
    return {
      newQweetContent: '',
      qweets: [
        {
          content:
            'Lorem ipsum dolor sit amet consectetur adipisicing elit Ducimus iste repellat, debitis molestiae obcaecati impedit? Deleniti culpa velit, rem iure voluptates, eum aspernatur dicta ipsam quo veritatis voluptate natus sequi?',
          date: 1701553581064
        },
        {
          content:
            'Lorem ipsum dolor sit amet consectetur adipisicing elit Ducimus iste repellat, debitis molestiae obcaecati impedit? Deleniti culpa velit, rem iure voluptates, eum aspernatur dicta ipsam quo veritatis voluptate natus sequi?',
          date: 1701553643064
        }
      ]
    }
  },
  methods: {
    addNewQweet() {
      let newQweet = {
        content: this.newQweetContent,
        date: Date.now()
      }
      this.qweets.unshift(newQweet)
      this.newQweetContent = ''
    },
    deleteQweet(qweet) {
      let dateToDelete = qweet.date
      let index = this.qweets.findIndex((qweet) => qweet.date === dateToDelete)
      // console.log('index', index)
      if (index !== -1) {
        this.qweets.splice(index, 1)
      }
    }
  },
  computed: {
    relativeDates() {
      return this.qweets.map((qweet) => formatDistance(qweet.date, new Date()))
    }
  }
}
</script>

<style lang="sass">
.new-qweet
    textarea
        font-size: 19px
        line-height: 1.4 !important
    .divider
        border-top: 1px solid
        border-bottom: 1px solid
        border-color: $grey-4
    .qweet-content
        white-space: pre-line
    .qweet-icons
      margin-left: -5px
</style>
