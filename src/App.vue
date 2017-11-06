<template>
    <div id="app" class="app">
        <h2 class="date">{{getFormattedDate}}</h2>

        <ul class="deeds">
            <li is="deed" @save="saveDeeds" v-for="(item, index) in deeds" v-bind:item="item"
                v-on:remove="removedeed(index)" v-bind:key="item.id"></li>
        </ul>
        <button v-on:click="addDeed" class="add-deed">+</button>
        <button @click="exportDeeds">Export</button>
        <pre id="export"></pre>
    </div>
</template>

<script>
    import Deed from './components/Deed.vue'

    export default {
        name: 'app',
        components: {
            deed: Deed
        },
        data() {
            return {
                todayDate: new Date(),
                deeds: []
            }
        },
        computed: {
           getFormattedDate() {
                return this.todayDate.toLocaleTimeString('ru-RU', {
                    day: 'numeric',
                    month: 'short',
                    weekday: 'short'
                })
           } 
        },
        mounted() {
            // this.deeds = deedsDone;
            const dateHash = new Date().setHours(0,0,0); // hash for day to store in localStorage

            if(localStorage.getItem('dateHash')) {
                const allDeeds = localStorage.getItem('dateHash');
                this.deeds = JSON.parse(allDeeds);
            } else {
                localStorage.setItem('dateHash', '[]')
            }

        },
        methods: {
            exportDeeds() {
                document.getElementById('export').innerHTML = localStorage.getItem('dateHash');
            },
            saveDeeds(event) {
                this.deeds.forEach(deed => {
                    deed.isEditable = false;
                });
                localStorage.setItem('dateHash', JSON.stringify(this.deeds))
            },
            addDeed(deed, i) {
                const index = this.deeds.length;
                const date  = new Date();

                this.deeds.push({
                    id: index,
                    date: +date,
                    title: '',
                    duration: 0,
                    isEditable: true
                });

                // set duration in previous deed
                if (index >= 1) {
                    const seconds = (+date - +this.deeds[index - 1].date) / 1000;
                    this.deeds[index - 1].duration = toHHMMSS(seconds)
                }
            },
            removedeed (index) {
                this.deeds.splice(index, 1);
                this.saveDeeds();
            }
        }
    }

    function toHHMMSS(secs) {
        console.log(secs);
        const sec_num = parseInt(secs, 10); // don't forget the second param

        let hours   = Math.floor(sec_num / 3600);
        let minutes = Math.floor((sec_num - (hours * 3600)) / 60);
        let seconds = sec_num - (hours * 3600) - (minutes * 60);

        if (hours > 0 && hours < 10) {
            hours = '0' + hours + ':';
        } else {
            hours = '';
        }

        if (minutes < 10) {
            minutes = '0' + minutes;
        }
        if (seconds < 10) {
            seconds = '0' + seconds;
        }

        return hours + minutes + ':' + seconds;
    }
</script>

<style lang="less">
    .app {
        max-width: 500px;
        margin: 20px auto;
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
        margin-top: 60px;
    }
    .date {
        text-align: center;
    }
    .add-deed {
        width: 100%;
        margin: 20px 0 0;
        padding: 20px;
    }
    #export {
        white-space: pre-line;
    }
</style>
