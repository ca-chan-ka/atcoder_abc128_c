<template>
    <div id="main">
        <textarea class="input" v-model="input"></textarea>
        <h2>Switch:</h2>
        <div class="switch">
            <div v-for="(i, index) in switches" :key="index" class="inner" :class="{ active: switches[index] == 1}" v-on:click="() => click(index)">{{ index + 1 }}</div>
        </div>
        <h2>Light:</h2>
        <div class="light">
            <div v-for="(i, index) in lights" :key="index" class="inner" :class="{ active: lights[index] == 1}">{{ index + 1 }}</div>
        </div>
    </div> 
</template>

<script>
export default {
    name: "Main",
    components: {  },
    data() {
        return {
            input: "",
            switches: new Array(10).fill(0),
            lights: new Array(10).fill(0),
        }
    },
    watch: {
        input(value) {
            const input = value
                .split("\n")
                .map(line => line.split(" ").map(character => character - 0));
            this.switches = new Array(input[0][0]).fill(0);
            this.refresh();
        }
    },
    methods: {
        click(value) {
            console.log(value);
            this.switches[value] = 1 - this.switches[value];
            this.refresh();
        },
        refresh() {
            const input = this.input
                .split("\n")
                .map(line => line.split(" ").map(character => character - 0));
            this.lights = new Array(input[0][1])
                .fill(0)
                .map((_, index) => input.slice(1)[index].slice(1).map(i => this.switches[i - 1]).filter(i => i == 1).length % 2 == input[input.length - 2][index]);
        },
    },
}
</script>

<style>
* * {
    margin: 0px;
    font-family: monospace;
    font-size: 16px;
    box-sizing: border-box;
    user-select: none;
}

@font-face {
    font-family: "Number";
    src: url("./assets/DINAlternate-Bold.ttf");
}

.input {
    width: 400px;
    height: 400px;
    resize: none;
    font-size: 32px;
    border: none;
    background-color: #fafafa;
    padding: 8px;
    font-family: Number;
}

.light {
    display: flex;
    user-select: none;
}

.light .inner {
    display: flex;
    width: 40px;
    height: 40px;
    background-color: #eee;
    align-items: center;
    justify-content: center;
    border-right: 1px solid #fff;
    border-radius: 20px;
}

.light .active {
    background-color: #faa;
}

.switch {
    display: flex;
    cursor: pointer;
}

.switch .inner:hover {
    background-color: #ddd;
}

.switch .inner {
    display: flex;
    width: 40px;
    height: 40px;
    background-color: #eee;
    align-items: center;
    justify-content: center;
    border-right: 1px solid #fff;
}

.switch .active {
    background-color: #afa;
}

.switch .active:hover {
    background-color: #ada;
}

.switch .disabled {
    background-color: #fff;
}

.switch .disabled:hover {
    background-color: #fff;
}

textarea {
    outline: none;
}

h2 {
    font-weight: normal;
    padding-top: 16px;
    padding-bottom: 4px;
}
</style>
