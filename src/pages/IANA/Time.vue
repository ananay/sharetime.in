<template>
  <section class="section">
    <div class="container has-text-centered">
      <div class="columns is-desktop is-centered content is-vcentered">
        <div class="is-narrow is-marginless">
          Time as provided
        </div>
        <h2 class="column is-narrow is-marginless">
          {{input.toFormat('HHmm')}}
        </h2>
        <div class="is-narrow is-marginless">
          in <b>{{input.zoneName}}</b> ({{input.offsetNameShort}}) is
        </div>
      </div>
      <div class="content">
        <h1 class="is-size-1">{{output.toFormat('hh:mm:ss a')}}</h1>
        <div>in your local timezone</div>
        <h3 class="is-marginless"><b>{{output.zoneName}}</b> ({{output.offsetNameShort}})</h3>
      </div>
    </div>
  </section>
</template>
<script lang="ts">
import { DateTime } from 'luxon'
import { Component, Vue } from 'vue-property-decorator'
import { Timer } from 'vue-plugin-timers'

interface RouteParams {
  continent: string
  city: string
  time: string
}

@Component
export default class Time extends Vue {
  rParams: RouteParams = this.$store.state.route.params
  timezone = this.rParams.continent + '/' + this.rParams.city
  input = DateTime.fromJSDate(new Date(), { zone: this.timezone })
  get output(): DateTime {
    return this.input.toLocal()
  }

  created() {
    this.input = DateTime.fromFormat(this.rParams.time, 'HHmm', {
      zone: this.timezone
    })
  }
}
</script>
