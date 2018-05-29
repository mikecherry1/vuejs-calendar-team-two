<template>
    <div>
        <button @click="decYear">&laquo;</button>
        <button @click="decMonth">&lsaquo;</button>
        <div class="date">{{ formattedDate }}</div>
        <button @click="incMonth">&rsaquo;</button>
        <button @click="incYear">&raquo;</button>
    </div>
</template>
<script>
    export default {
        methods: {
            jumpToPresent() {
                let month = this.$moment().month() + 1;
                let year = this.$moment().year();
                this.$store.commit('setCurrentMonth', month);
                this.$store.commit('setCurrentYear', year);
            },
            decMonth() {
                if (this.month === 1) {
                    this.$store.commit('setCurrentMonth', 12);
                    this.$store.commit('setCurrentYear', this.year - 1);
                } else {
                    this.$store.commit('setCurrentMonth', this.month - 1);
                }
                this.$store.commit('eventFormActive', false);
            },
            decYear() {
                this.$store.commit('setCurrentYear', this.year - 1);
                this.$store.commit('eventFormActive', false);
            },
            incMonth() {
                if (this.month === 12) {
                    this.$store.commit('setCurrentMonth', 1);
                    this.$store.commit('setCurrentYear', this.year + 1);
                } else {
                    this.$store.commit('setCurrentMonth', this.month + 1);
                }
                this.$store.commit('eventFormActive', false);
            },
            incYear() {
                this.$store.commit('setCurrentYear', this.year + 1);
                this.$store.commit('eventFormActive', false);
            }
        },
        computed: {
            formattedDate() {
                return this.$moment(`${this.year}-${this.month}-1`, 'YYYY-M-D').format('MMMM YYYY');
            },
            month() {
                return this.$store.state.currentMonth;
            },
            year() {
                return this.$store.state.currentYear;
            },
        }
    }
</script>
