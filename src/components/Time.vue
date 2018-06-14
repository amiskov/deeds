<template>
    <div>
    <time :datetime="date" v-if="!isEditable" @click="changeTime" class="time">
        {{formattedTime}}
    </time>
    <div v-else>
        <input v-focus @blur="updateTime" type="time" :value="formattedTime">
    </div>
    </div>
</template>
<script>
import {EventBus} from '../main'

export default {
    props: ['date'],
    data() {
        return {
            isEditable: false
        }
    },
    computed: {
        formattedTime() {
            const dateOptions = {
                hour: '2-digit',
                minute: '2-digit'
            };

            return new Date(this.date).toLocaleString('ru-RU', dateOptions)
        },
    },
    methods: {
        changeTime() {
            this.isEditable = true;
        },
        updateTime(ev) {
            this.isEditable = false;
            this.$emit('timeUpdated', ev.target.value);
        }
    },
    directives: {
        focus: {
            inserted: function (el) {
                el.focus();
            }
        }
    }
}    
</script>
<style lang="less">
    .time {
        line-height: 3em;
        margin: 0 .5em;
    }
</style>