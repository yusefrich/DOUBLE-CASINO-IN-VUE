<template>
    <div class="container d-flex aling-items-center justify-content-center mt-5 w-100">
        <div>
            <div class="app">
                <div class="scopeHidden">
                    <ul ref="list">
                        <li v-for="(value, index) in numbers" :key="index" :class="[(index % 2) ? 'middle' : '',
                        (value >= 1 && value <= 7) ? 'bg-red' : '',
                        (value >= 8 && value <= 14) ? 'bg-black' : '',
                        (value === 15) ? 'bg-white' : '']" ref="listItem">
                            {{ value }}
                        </li>
                    </ul>
                </div>
            </div>
            <button class="btn" @click="start()">Girar</button>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            numbers: Array.from({ length: 25 }, () => Math.floor(Math.random() * 15) + 1)
        }
    },
    mounted() {
        this.list = this.$refs.list;
        this.listItem = this.$refs.listItem[0];
    },
    methods: {
        start() {
            this.numbers = Array.from({ length: 25 }, () => Math.floor(Math.random() * 15) + 1);

            const move = -150 * 15;
            this.list.style.left = move + 'px';

            const index = -Math.floor((move + (this.$refs.list.offsetWidth / 2) / -150) / 150) + 1;
            if (index >= 0 && index < this.$refs.listItem.length) {
                this.$refs.listItem[index].classList.add('this-list');
            }
            console.log(this.numbers[index]);

            // resetando a roleta
            setTimeout(() => {
                this.list.style.left = '0';
                this.$refs.listItem.forEach(li => li.style.background = '');
            }, 15000);
        }
    }
}
</script>

<style scoped>
/* body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

* {
    margin: 0;
    padding: 0;
    list-style: none;
} */

.app {
    color: #e8e8e8;
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    font-size: 20px;
}
.this-list {
    border: 3px solid #00ff1a;
}

.bg-white {
    background-color: #fff;
    color: #000;
}

.bg-red {
    background-color: #ff0000;
}

.bg-black {
    background-color: #000;
}

.app>.btn {
    position: relative;
    left: 50%;
    transform: translate(-50%, 20px);
    padding: 10px 30px;

    font-size: 15px;
    font-weight: 600;
    letter-spacing: 1px;
    background: #4c4c4c;
    color: white;

    cursor: pointer;
    transition: 0.2s ease;
}

.app>.btn:hover {
    background: #2f2f2f;
    letter-spacing: 3px;
    box-shadow: 0px 0px 25px rgba(0, 0, 0, 0.5);
}

.scopeHidden {
    overflow: hidden;
    width: 800px;
    height: 150px;
    background: #2f2f2f;
    border-radius: 10px;
    box-shadow: 0px 0px 25px rgba(0, 0, 0, 0.5);
}

.scopeHidden>ul {
    position: relative;
    /*

  left: 0; Тоже укажите, иначе transition не будет работать

  */
    left: 0;
    display: inline-flex;
    transition: 3s ease;
}

.scopeHidden>ul>li {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 150px;
    height: 150px;
}

.scopeHidden>ul>li.middle {
    /* background: #000000; */
}
</style>