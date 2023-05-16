<script setup>
import { ref, onMounted } from 'vue'
import { Countdown } from 'vue3-flip-countdown'

const messages = ref([])
const userInput = ref('')

var timeSince = function (date) {
    if (typeof date !== 'object') {
        date = new Date(date);
    }

    var seconds = Math.floor((new Date() - date) / 1000);
    var intervalType;

    var interval = Math.floor(seconds / 31536000);
    if (interval >= 1) {
        intervalType = 'year';
    } else {
        interval = Math.floor(seconds / 2592000);
        if (interval >= 1) {
            intervalType = 'month';
        } else {
            interval = Math.floor(seconds / 86400);
            if (interval >= 1) {
                intervalType = 'day';
            } else {
                interval = Math.floor(seconds / 3600);
                if (interval >= 1) {
                    intervalType = "hour";
                } else {
                    interval = Math.floor(seconds / 60);
                    if (interval >= 1) {
                        intervalType = "minute";
                    } else {
                        interval = seconds;
                        intervalType = "second";
                    }
                }
            }
        }
    }

    if (interval > 1 || interval === 0) {
        intervalType += 's';
    }

    return interval + ' ' + intervalType;
};

const send = () => {
    const message = {
        id: Math.floor((Math.random() * 100)),
        content: userInput.value,
        time: Date(),
    }

    messages.value.push(message)
    userInput.value = null
}

const timeElapsedd = () => {
    console.log("Time up!");
    messages.value = null
}

onMounted(() => {
    userInput.value.focus
})
</script>

<template>
    <q-page class="flex column">
        <Countdown :timeElapsed="timeElapsedd" class="q-pa-md" fixed-top deadline="2023-05-16 16:37:00" :showDays="false" />
        <div class="q-pa-md col column justify-end">
            <q-list bordered separator full-height>
                <q-item v-ripple v-for="msg in messages" :key="msg.id">
                    <q-item-section>
                        <q-item-label>{{ msg.content }}</q-item-label>
                        <q-item-label caption lines="2">Secondary line text. Lorem ipsum dolor sit amet, consectetur
                            adipiscit elit.</q-item-label>
                    </q-item-section>

                    <q-item-section side top>
                        <q-item-label caption>3 mins ago</q-item-label>
                    </q-item-section>
                </q-item>
            </q-list>
        </div>

        <q-footer elevated class="q-pa-md">
            <q-input type="text" bg-color="white" rounded outlined dense flat full-width autofocus autogrow
                v-model="userInput">
                <template v-slot:after>
                    <q-btn round dense color="white" flat icon="send" @click="send" />
                </template></q-input>
        </q-footer>
    </q-page>
</template>


