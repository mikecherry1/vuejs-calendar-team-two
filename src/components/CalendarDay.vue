<template>
    <div :class="classObject" @click="captureClick">
        <div v-if="fromCurrentMonth">{{ day.format('D') }}</div>
        <div class="days-from-other-months" v-else>{{ day.format('D') }}</div>
        <ul class="event-list">
            <li v-for="event in events" :key="event.id">{{ event.description }}</li>
        </ul>
    </div>
</template>
<script>
    export default {
        props: [ 'day' ],
        data() {
            return {
                fromCurrentMonth: false
            }
        },
        computed: {
            events() {
                return this.$store.state.events.filter(event => event.date.isSame(this.day, 'day'));
            },
            classObject() {
                let eventFormDate = this.$store.state.eventFormDate;
                let eventFormActive = this.$store.state.eventFormActive;
                let today = this.day.isSame(this.$moment(), 'day');

                this.fromCurrentMonth = ((this.day.month() + 1) === this.month)
                return {
                    day: true,
                    today,
                    past: this.day.isSameOrBefore(this.$moment(), 'day') && !today,
                    active: eventFormDate.isSame(this.day, 'day') && eventFormActive,
                };
            },
            month() {
                return this.$store.state.currentMonth;
            },
        },
        methods: {
            captureClick(event) {
                this.$store.commit('eventFormPos', { x: event.clientX, y: event.clientY });
                this.$store.commit('eventFormActive', true);
                this.$store.commit('eventFormDate', this.day);
            },
        }
    }
</script>
