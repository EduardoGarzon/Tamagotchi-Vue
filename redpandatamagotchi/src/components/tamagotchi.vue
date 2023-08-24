<template>
    <img :src="Humor" alt="PandaSVG" class="panda">
    <Actions @humorUpdate="currentlyHumor"></Actions>
</template>

<script>
import Actions from './actions.vue';

export default {
    components: {
        Actions,
    },
    data() {
        return {
            humor: '',
        }
    },
    methods: {
        currentlyHumor(newHumor) {
            this.humor = newHumor;
        }
    },
    computed: {
        Humor() {
            if (this.humor === 'Angry') {
                return '../../images/angrypanda.svg';
            } else if (this.humor === 'Hungry') {
                return '../../images/angrypanda.svg';
            } else if (this.humor === 'Tired') {
                return '../../images/sadpanda.svg';
            } else if (this.humor === 'Happy') {
                return '../../images/happypanda.svg';
            } else if (this.humor === 'Sad') {
                return '../../images/sadpanda.svg';
            } else if (this.humor === 'Sleeping') {
                return '../../images/sleepingpanda.svg';
            } else if (this.Humor === 'Playing') {
                return '../../images/playingpanda.svg';
            } else {
                return '../../images/normalpanda.svg';
            }
        },
    },
};
</script>

<style>
.panda {
    max-width: 150px;
    position: absolute;
    left: 81px;
    top: 125px;
}
</style>
  