<script>
import { RouterLink, RouterView } from 'vue-router'
import { reactive, toRefs } from 'vue';
import Device from './components/Device.vue'
import tamagotchi from './components/tamagotchi.vue'
import Actions from './components/actions.vue'

export default {
  name: 'Home',
  components: {
    Device,
    tamagotchi,
    //Actions,
  },
  setup() {
    const state = reactive({
      petname: '',
      userinput: '',
      showCreateTamagotchi: true,
    })

    const createTamagotchi = () => {
      if (state.userinput.length == 0) {
        window.alert("Informe um nome para o Tamagotchi!")
      } else {
        state.petname = state.userinput,
          state.showCreateTamagotchi = false
      }
    }
    return {
      ...toRefs(state),
      createTamagotchi
    }
  },
}

</script>

<template>
  <h1>{{ petname }}</h1>
  <section :class="$style.wrapper">
    <Device />
    <tamagotchi v-if="petname" />
  </section>  

  <form v-if="showCreateTamagotchi" @submit.prevent>
    <input v-model="userinput" type="text" placeholder="Tamagotchi Name" id="petname">
    <button @click="createTamagotchi" type="button" class="btn btn-success">Create PET
    </button>
  </form>
  <RouterView />
</template>

<style module>
.wrapper {
  position: relative;
}

h1 {
  text-shadow: 0px 0px 1px white;
  font-family: monospace;
  font-weight: 700;
  text-transform: capitalize;
  text-align: center;
}

form {
  margin-top: 20px;
}

form button {
  margin-left: 10px;
  text-shadow: 0px 0px 3px black;
}
</style>
