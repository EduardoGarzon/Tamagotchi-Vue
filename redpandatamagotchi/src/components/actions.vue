<template>
    <div class="message" v-bind:class="getClassColor()">
        <h2>AÇÕES:</h2>
        <p><span v-if="showMessage">{{ messageText }}</span></p>
        <p><span>{{ messageText2 }}</span></p>
    </div>
    <div class="status">
        <h2>STATUS:</h2>
        <p>Energy: <span v-bind:class="getClassEnergia()">{{ energy }}</span> </p>
        <p>Happiness: <span v-bind:class="getClassHappiness()">{{ happiness }}</span> </p>
        <p>Hunger: <span v-bind:class="getClassHunger()">{{ hunger }}</span> </p>
        <p>Humor: <span v-bind:class="getClassHumor()">{{ humor }}</span> </p>
    </div>
    <div class="actions">
        <button type="button" class="btn btn-primary" @click="feed">Feed</button>
        <button type="button" class="btn btn-primary" @click="play">Play</button>
        <button type="button" class="btn btn-primary" @click="sleep">Sleep</button>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            energy: 60,
            happiness: 60,
            hunger: 40,
            humor: "Neutro",
            intervalID: null,
            showMessage: true,
            messageText: "Pet Criado!",
        };
    },
    methods: {
        getClassColor() {
            if (this.humor == "Happy") {
                return 'border-happy';
            } else if (this.humor == "Angry") {
                return 'border-angry';
            } else if (this.humor == "Neutro") {
                return 'border-neutro';
            } else if (this.humor == "Sad") {
                return 'border-sad';
            } else if (this.humor == "Tired") {
                return 'border-tired';
            } else if (this.humor == "Sleeping") {
                return 'border-sleeping';
            } else if (this.humor == "Hungry") {
                return 'border-hungry';
            }
        },
        getClassEnergia() {
            if (this.energy <= 30) {
                return 'energia-baixa';
            } else if (this.energy <= 70) {
                return 'energia-media';
            } else {
                return 'energia-alta';
            }
        },
        getClassHappiness() {
            if (this.happiness < 30) {
                return 'hapinnes-baixo';
            } else if (this.happiness < 50) {
                return 'hapinnes-medio';
            } else if (this.happiness < 70) {
                return 'hapinnes-ok';
            } else {
                return 'hapinnes-alto';
            }
        },
        getClassHunger() {
            if (this.hunger <= 20) {
                return 'hunger-baixo';
            } else if (this.hunger <= 60) {
                return 'hunger-medio';
            } else if (this.hunger <= 70) {
                return 'hunger-alto';
            } else {
                return 'starving';
            }
        },
        getClassHumor() {
            if (this.humor == "Angry") {
                return 'angry-humor';
            } else if (this.humor == "Neutro") {
                return 'normal-humor';
            } else if (this.humor == "Happy") {
                return 'happy-humor';
            } else if (this.humor == "Hungry") {
                return 'hungry-humor';
            } else if (this.humor == "Tired") {
                return 'tired-humor';
            } else if (this.humor == "Sad") {
                return 'sad-humor';
            } else if (this.humor == "Sleeping") {
                return 'sleeping-humor';
            }
        },
        checkStatusFeed() {
            // pode estar angry, hungry, tired, sad e happy
            if (this.hunger > 70) {
                this.humor = "Angry";
                this.setMessage("O Tamagotchi esta Bravo!");
            } else if (this.hunger <= 70 && this.hunger >= 60) {
                this.humor = "Hungry";
                this.setMessage("O Tamagotchi esta com Fome!");
            } else if (this.energy <= 30) {
                this.humor = "Tired";
                this.setMessage("O Tamagotchi esta Cansado!");
            } else if (this.happiness <= 30) {
                this.humor = "Sad";
                this.setMessage("O Tamagotchi esta Triste!");
            } else {
                this.humor = "Happy";
                this.setMessage("O Tamagotchi esta Feliz!");
            }
            this.$emit('humorUpdate', this.humor);
        },
        feed() {
            if (this.hunger > 0) {
                if (this.energy == 0) {
                    window.alert("TAMAGOTCHI ESTA SEM ENERGIA PARA COMER, FACA-O DORMIR!");
                }
                else {
                    if (this.hunger - 20 < 0) {
                        this.hunger = 0;
                    } else {
                        this.hunger -= 20;
                    }
                    this.energy -= 10;
                }
                this.checkStatusFeed();
            } else {
                window.alert("TAMAGOTCHI NAO ESTA COM FOME!");
            }
        },
        checkStatusPlay() {
            // pode ficar neutro, feliz, com fome e cansado
            if (this.hunger <= 70 && this.hunger >= 60) {
                this.humor = "Hungry";
                this.setMessage("O Tamagotchi esta com Fome!");
            } else if (this.energy <= 30) {
                this.humor = "Tired";
                this.setMessage("O Tamagotchi esta Cansado!");
            } else if (this.happiness >= 70) {
                this.humor = "Happy";
                this.setMessage("O Tamagotchi esta Feliz!");
            }

            this.$emit('humorUpdate', this.humor);
        },
        play() {
            if (this.happiness == 100) {
                window.alert("TAMAGOCHI NAO PRECISA BRINCAR!");
            } else if (this.energy > 30) {
                if (this.hunger >= 60) {
                    window.alert("TAMAGOTCHI ESTA COM FOME!");
                }
                else {

                    if (this.happiness + 20 > 100) {
                        this.happiness = 100;
                    } else {
                        this.happiness += 20;
                    }
                    this.energy -= 20;
                    this.hunger += 20;
                    if (this.energy <= 30) {
                        window.alert("TAMAGOTCHI ESTA SEM ENERGIA APÓS BRINCAR!");
                    }
                }
                this.checkStatusPlay();
            } else {
                window.alert("TAMAGOTCHI ESTA SEM ENERGIA, FACA-O DORMIR!");
            }
        },
        checkStatusSleep() {
            // pode acordar neutro, triste, feliz ou com fome
            if (this.hunger > 70) {
                this.humor = "Angry";
                this.setMessage("O Tamagotchi esta Bravo!");
            } else if (this.hunger > 50) { // Hungry
                this.humor = "Hungry";
                this.setMessage("O Tamagotchi esta com Fome!");
            } else if (this.happiness <= 30) {                   // Sad
                this.humor = "Sad";
                this.setMessage("O Tamagotchi esta Triste!");
            } else if (this.happiness >= 70) {                   // Happy
                this.humor = "Happy";
                this.setMessage("O Tamagotchi esta Feliz!");
            } else {
                this.humor = "Neutro";
                this.setMessage("O Tamagotchi esta Neutro!");
            }
            this.$emit('humorUpdate', this.humor);
        },
        sleep() {
            if (this.energy > 60) {
                window.alert("TAMAGOTCHI NAO PRECISA DORMIR!");
            } else if (this.energy <= 60) {
                const resposta = confirm("TAMOGOTCHI IRÁ DORMIR...");
                if (resposta == true) {
                    if (this.intervalId) {
                        clearInterval(this.intervalId);
                        this.intervalId = null;
                    } else {
                        this.humor = "Sleeping";
                        let tamagotchiRecarregado = false;
                        let count = 0;
                        this.$emit('humorUpdate', this.humor);
                        this.setMessage("Tamagotchi esta dormindo!"); 
                        this.getClassColor();
                        this.intervalId = setInterval(() => {
                            this.energy += 10;
                            count++;
                            if (count == 2) {
                                if (this.hunger < 100) {
                                    this.hunger += 10;
                                } else if (this.hunger == 100) {
                                    this.hunger = 100;
                                }
                                count = 0;
                            }
                            if (this.energy >= 100) {
                                this.energy = 100;
                                clearInterval(this.intervalId);
                                this.intervalId = null;
                                tamagotchiRecarregado = true;
                                // Usar setTimeout para exibir o alerta após a última iteração
                                setTimeout(() => {
                                    if (tamagotchiRecarregado) {
                                        if (this.hunger > 70) {
                                            window.alert("TAMAGOTCHI ACORDOU FAMINTO!");
                                        } else if (this.hunger <= 70 && this.hunger >= 60) {
                                            window.alert("TAMAGOTCHI ACORDOU COM FOME!");
                                        }
                                        if (this.happiness > 0) {
                                            this.happiness -= 30;
                                        } else if (this.happiness <= 0) {
                                            this.happiness = 0;
                                        }
                                        if (this.happiness <= 0) {
                                            window.alert("TAMAGOTCHI ACORDOU TRISTE!");
                                        }
                                        window.alert("TAMAGOTCHI RECARREGADO 100%!");
                                        tamagotchiRecarregado = false; // Resetar a variável para futuras recargas
                                        this.checkStatusSleep();
                                    }
                                }, 0); // 0ms para que o alerta seja colocado na fila de tarefas
                            }
                        }, 1000); // 1000ms = 1 segundo
                    }

                } else {
                    window.alert("TAMAGOTCHI ACORDADO!");
                }
            }
        },
        setMessage(newMessage) {
            this.messageText = newMessage;
        },
    }
};
</script>
  
<style>

.message {
    position: absolute;
    right: 400px;
    top: 0px;
    display: flex;
    flex-direction: column;
    font-family: monospace;
    width: 400px;
    height: 370px;
    border: 5px solid white;
    align-items: center;
    justify-content: center;
    box-shadow: 0px 0px 5px white;
}

.message p {
    text-align: center;
    white-space: nowrap;
    display: flex;
    flex-direction: row;
    text-shadow: 0px 0px 1px white;
    font-size: 20px;
    font-weight: 700;
    text-shadow: 0px 0px 2px white;
}

.message h2 {
    position: absolute;
    margin-bottom: 30px;
    text-shadow: 0px 0px 1px white;
    font-weight: 700;
    top: 10px;
    left: 150px;
}

.status {
    position: absolute;
    left: 450px;
    top: 0;
    display: flex;
    flex-direction: column;
    font-family: monospace;

}

.status p {
    text-align: left;
    text-shadow: 0px 0px 1px white;
    font-size: 20px;
    font-weight: 700;
    display: flex;
    flex-direction: row;
}

.status h2 {
    margin-bottom: 30px;
    text-shadow: 0px 0px 1px white;
    font-weight: 700;
}

.actions {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
}

.actions button {
    margin-left: 10px;
    margin-top: 10px;
}

/* Cores de indentificacao para a energia do tamagotchi */
.energia-baixa {
    color: red;
}

.energia-media {
    color: orange;
}

.energia-alta {
    color: green;
}

/* Cores de identificacao para a felicidade do tamagotchi */
.hapinnes-baixo {
    color: red;
}

.hapinnes-medio {
    color: yellow;
}

.hapinnes-ok {
    color: orange;
}

.hapinnes-alto {
    color: green;
}

/* Cores de identificacao para a fome do tamagotchi */
.hunger-baixo {
    color: green;
}

.hunger-medio {
    color: yellow;
}

.hunger-alto {
    color: orange;
}

.starving {
    color: red;
}

/* Cores de identificacao para o humor do tamagotchi */
.angry-humor {
    color: red;
}

.normal-humor {
    color: white;
}

.happy-humor {
    color: green;
}

.hungry-humor {
    color: orange;
}

.tired-humor {
    color: rgb(161, 76, 76);
}

.sad-humor {
    color: grey;
}

.sleeping-humor {
    color: rgb(82, 82, 201);
}

/* Cores da borda da caixa de acoes */
.border-happy {
    border-color: green;
}

.border-angry {
    border-color: red;
}

.border-sad {
    border-color: gray;
}

.border-hungry {
    border-color: orange;
}

.border-tired {
    border-color: rgb(161, 76, 76);
}

.border-sleeping {
    border-color: rgb(82, 82, 201);
}

.border-normal {
    border-color: white;
}
</style>
  