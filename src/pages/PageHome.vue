<template>
  <q-page class="relative-position">
     <q-scroll-area class="absolute fullscreen">
      <div class="q-py-lg q-px-md row items-end q-col-gutter-md">
        <div class="col">
            <q-input
              class="new-qweet"
              bottom-slots 
              v-model="newQweetContent" 
              placeholder="What‘s happening?" 
              counter 
              maxlength="280"
              autogrow
            >
              <template v-slot:before>
                <q-avatar size="xl">
                  <img src="https://cdn.quasar.dev/img/avatar5.jpg">
                </q-avatar>
              </template>

            </q-input>
        </div>
        <div class="col col-shrink">
                <q-btn
                @click="addNewQweet"
                class="q-mb-lg"
                  unelevated 
                  rounded 
                  color="primary" 
                  label="Qweet" 
                  no-caps
                  :disable="!newQweetContent"
                />
        </div>
      </div>

      <q-separator 
        class="divider"
        size="10px" 
        color="grey-2" 
      />
        <q-list separator>
          <transition-group
            appear
            enter-active-class="animated fadeIn slow"
            leave-active-class="animated fadeOut slow"
          >
            <q-item 
              v-for="qweet in qweets"
              :key="qweet.date"
              class="qweet q-py-md"
            >
              <q-item-section avatar top>
              <q-avatar size="xl">
                      <img src="https://cdn.quasar.dev/img/avatar5.jpg">
                </q-avatar>
              </q-item-section>

              <q-item-section>
                <q-item-label class="text-subtitle1">
                <strong>Christina Lee</strong> 
                <span class="text-grey-7">
                  @chris_50719
                  <br class="lt-md">&bull;  {{qweet.date | relativeDate}}
                </span>
                </q-item-label>
                <q-item-label class="qweet-content text-body2">{{qweet.content}}
                </q-item-label>
                <div class="qweet-icons row justify-between q-mt-sm">
                    <q-btn 
                      flat 
                      round 
                      color="grey" 
                      icon="far fa-comment"
                      size="sm" 
                    />
                    <q-btn 
                      flat 
                      round 
                      color="grey" 
                      icon="fas fa-retweet"
                      size="sm" 
                    />
                    <q-btn 
                      flat 
                      round 
                      color="grey" 
                      icon="far fa-heart"
                      size="sm" 
                    />
                    <q-btn
                      @click="deleteQweet(qweet)"
                      flat 
                      round 
                      color="grey" 
                      icon="fas fa-trash"
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
  data(){
    return{
      newQweetContent: '',
      qweets: [
        {
          content: 'Ill be in your neighborhood doing errands this weekend. Do you want to grab brunch?',
          date: 1621057374868
        },
        {
          content: 'Ill be in your neighborhood doing errands this weekend. Do you want to grab brunch?',
          date: 1621057436304
        },
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
      console.log('delete qweet:', qweet)
      let dateToDelete = qweet.date
      let index = this.qweets.findIndex(qweet => qweet.date === dateToDelete)
      this.qweets.splice(index, 1)
    }
  },
  filters: {
    relativeDate(value) {
      return formatDistance(value, new Date())
    }
  }
}
</script>

<style lang="sass">
  .new-qweet
    textarea
      font-size: 19px
      line-height: 1.4
  .divider
    border-top: 1px solid
    border-bottom: 1px solid
    border-color: $grey-4
  .qweet:not(:first-child)
    border-top: 1px solid rgba(0, 0, 0, 0.12)
  .qweet-content
    white-space: pre-line
  .qweet-icons
    margin-left: -5px
 
</style>
