<template>
    <div>
        <h1>Vue Simple Chatbot</h1>
        <div class="chat-box" ref="chatBox">
            <div v-for="message in messages" :key="message.id" class="message">
                <div v-if="message.type === 'user'" class="user-flex">
                    <span class="user-message">{{ message.text }}</span>
                    <img class="user-image" src="./assets/user.jpg" />
                </div>
                <div v-if="message.type === 'bot'" class="bot-flex">
                    <img class="bot-image" src="./assets/bot.jpg" />
                    <span class="bot-message">{{ message.text }}</span>
                </div>
            </div>
        </div>
        <div class="boxes">
            <div class="input-box">
                <input
                    v-model="inputText"
                    type="text"
                    @keypress.enter="sendMessage"
                />
                <button @click="sendMessage">Отправить</button>
            </div>
            <div class="button-box">
                <button
                    v-for="button in buttons"
                    :key="button.id"
                    @click="sendButtonMessage(button.text)"
                >
                    {{ button.text }}
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            inputText: '',
            messages: [],
            buttons: [
                { id: 1, text: 'Привет' },
                { id: 2, text: 'Как дела?' },
                { id: 3, text: 'Пока' },
            ],
        }
    },
    methods: {
        sendMessage() {
            if (this.inputText.trim() !== '') {
                this.addUserMessage(this.inputText)
                this.inputText = ''
            }
        },
        sendButtonMessage(text) {
            this.addUserMessage(text)
        },
        addUserMessage(text) {
            this.messages.push({ id: Date.now(), text: text, type: 'user' })
            this.scrollToBottom()
            this.processMessage(text)
        },
        addBotMessage(text) {
            this.messages.push({ id: Date.now(), text: text, type: 'bot' })
            this.scrollToBottom()
        },
        processMessage(text) {
            // Здесь можно добавить логику для обработки запросов и генерации ответов бота
            if (text === 'Привет') {
                setTimeout(() => {
                    this.addBotMessage('Привет! Как я могу вам помочь?')
                }, 1000)
            } else if (text === 'Как дела?') {
                setTimeout(() => {
                    this.addBotMessage('У меня все отлично, спасибо!')
                }, 1000)
            } else if (text === 'Пока') {
                setTimeout(() => {
                    this.addBotMessage('До свидания!')
                }, 1000)
            } else {
                setTimeout(() => {
                    this.addBotMessage('Извините, я не понял вашего сообщения.')
                }, 1000)
            }
        },
        scrollToBottom() {
            setTimeout(() => {
                this.$refs.chatBox.scrollTop = this.$refs.chatBox.scrollHeight
            }, 50)
        },
    },
}
</script>

<style lang="scss">
@mixin border {
    -webkit-box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
    -moz-box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
    box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
}
@mixin image {
    background-repeat: no-repeat;
    height: 50px;
    width: 50px;
    align-self: center;
    @include border();
    padding: 2px;
}
@mixin button {
    border: none;
    border-radius: 10px;
    padding: 10px 15px 10px 15px;
    width: 120px;
    cursor: pointer;
    color: white;
    font-weight: bold;
}
h1 {
    font-weight: bold;
    text-align: center;
    margin-top: 0;
}
body {
    background-color: rgb(244, 228, 149);
    padding-top: 50px;
    font-family: Arial, sans-serif;
}
.chat-box {
    height: 300px;
    overflow-y: scroll;
    outline: 1px solid #ccc;
    padding: 10px;
    background-color: white;
    width: 400px;
    height: 600px;
    border-radius: 10px 10px 0 0;
    &::-webkit-scrollbar {
        width: 0;
    }
}
.boxes {
    background-color: orange;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 8px;
    outline: 1px solid #ccc;
    border-radius: 0 0 10px 10px;
}
.message {
    margin-bottom: 10px;
}
.user-message {
    text-align: right;
    background: white;
    color: purple;
    display: inline-block;
    padding: 15px;
    border-radius: 20px 20px 0 20px;
    font-weight: bold;
    @include border();
}
.user-image {
    @include image();
    border-radius: 50% 50% 50% 0;
    margin-left: 10px;
}
.bot-image {
    @include image();
    border-radius: 50% 50% 0 50%;
    margin-right: 10px;
}
.bot-message {
    text-align: left;
    background: purple;
    color: white;
    display: flex;
    padding: 15px;
    border-radius: 20px 20px 20px 0;
    img {
        background-repeat: no-repeat;
        width: 50px;
        height: 50px;
    }
}
.input-box {
    // margin-top: 10px;
    display: flex;
    justify-content: space-between;
    input {
        width: 265px;
        border-radius: 10px;
        border: none;
        outline: 1px solid rgb(181, 179, 179);
    }
    button {
        @include button();
        background-color: red;
    }
}
.button-box {
    margin-top: 10px;
    display: flex;
    justify-content: space-between;
    button {
        @include button();
        background-color: blueviolet;
    }
}
button {
    margin-right: 5px;
}

.user-flex {
    display: flex;
    align-items: end;
    justify-content: flex-end;
    font-weight: bold;
}
.bot-flex {
    display: flex;
    align-items: end;
    justify-content: flex-start;
    font-weight: bold;
}
</style>
