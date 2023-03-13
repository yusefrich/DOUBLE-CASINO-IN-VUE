<template>
    <div class="container d-flex aling-items-center justify-content-center w-100">
        <div>
            <div class="app">
                <div class="scopeHidden">
                    <ul>
                        <li v-for="(value, index) in numbers" :class="[(index % 2) ? 'middle' : '']">
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
            numbers: Array.from({ length: 25 }, () => Math.floor(Math.random() * 14))
        }
    },
    methods: {
        start() {
            this.numbers = Array.from({ length: 25 }, () => Math.floor(Math.random() * 14));

            const move = -150 * 15;
            const ul = document.querySelector('.scopeHidden > ul');
            ul.style.left = move + 'px';

            const index = -Math.floor((move + (document.querySelector('.scopeHidden').offsetWidth / 2) / -150) / 150) + 1;
            const li = document.querySelectorAll('.scopeHidden > ul > li')[index];
            li.style.background = 'red';

            // resetando a roleta
            setTimeout(() => {
                ul.style.left = '0';
                document.querySelectorAll('.scopeHidden > ul > li').forEach(li => li.style.background = '');
            }, 10000);
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
    background: #282828;
}
</style>