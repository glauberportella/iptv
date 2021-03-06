<template>
  <div class="container">
    <table class="highlight">
      <thead>
        <tr>
          <th>Título</th>
          <th>Canais</th>
          <th v-if="hasChannelViewers">Audiência</th>
        </tr>
      </thead>

      <tbody
        v-for="program in sortedCurrentPrograms"
        v-if="program && channelMap[program.channel]"
        :key="program.channel"
        @click="switchChannel(program.channel)">
        <tr>
          <td>{{ program.title }}</td>
          <td>{{ channelMap[program.channel].Name }}</td>
          <td v-if="hasChannelViewers">{{ program.viewers }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import fuzzy from 'fuzzy';
import {mapState, mapGetters} from 'vuex';
import {channelLink} from '../route/link.js';
import config from '../../config.json5';

export default {
  name: 'ProgramList',
  props: {
    filter: String,
  },
  data() {
    return {
      hasChannelViewers: !!config.channelViewersUrl,
    };
  },
  computed: {
    currentPrograms() {
      const programs = this.$store.state.epg;
      return Object.entries(programs).map(([channel, programs]) => {
        const currentProgram = programs.find((program) => {
          return program.start < this.now && program.stop > this.now;
        });
        if (currentProgram) {
          currentProgram.channel = channel;
          currentProgram.viewers = this.getViewers(channel);
          return currentProgram;
        }
      });
    },
    filteredCurrentPrograms() {
      return this.currentPrograms.filter((program) => {
        return program && (!this.filter ||
          fuzzy.test(this.filter, program.channel) ||
          fuzzy.test(this.filter, program.title));
      });
    },
    sortedCurrentPrograms() {
      return this.filteredCurrentPrograms.slice(0).sort((a, b) => {
        return (this.hasChannelViewers && b.viewers - a.viewers)
          || a.channel.localeCompare(b.channel);
      });
    },
    ...mapState([
      'now',
    ]),
    ...mapGetters([
      'channelMap',
      'getViewers',
    ]),
  },
  methods: {
    switchChannel(channel) {
      // TODO: 2nd screen
      this.$router.push(channelLink(channel, this.$route.name));
    },
  },
};
</script>
