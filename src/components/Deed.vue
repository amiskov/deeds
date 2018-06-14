<template>
    <li class="deed">
        <deed-time @timeUpdated="updateTime" :date="item.date"></deed-time>

        <div class="deed__text" @click="isEditable = true">
            <span class="deed__title" v-if="!isEditable">{{item.title}}</span>
            <input v-focus v-if="isEditable" class="deed__edit"
                   @blur="saveDeed"
                   @keyup.enter="saveDeed"
                   type="text" v-model="item.title">
        </div>

        <div class="deed__actions">
            <span v-if="item.duration !== 0" class="deed__duration">{{item.duration}}</span>
            <button class="deed__remove" v-on:click="remove">&times;</button>
        </div>
    </li>
</template>

<script>
    import Time from './Time.vue'

    export default {
        name: 'deed',
        components: {
            'deed-time': Time
        },
        props: {
            item: {
                type: Object,
                default: function () {
                    return []
                }
            }
        },
        data() {
            return {
                isEditable: this.item.isEditable
            }
        },
        methods: {
            saveDeed() {
                this.isEditable = false
                this.$emit('save')
            },
            updateTime(data) {
                let date = new Date(this.item.date);
                const hours = data.split(':')[0];
                const mins = data.split(':')[1];

                this.$emit('save', {
                    deedId: this.item.id,
                    newDate: +date.setHours(hours, mins)
                });
            },
            remove: function () {
                this.$emit('remove');
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
    .deeds {
        margin: 0;
        padding: 0;
        list-style-type: none;
    }

    .deed {
        border-bottom: 1px solid #ccc;
        display: flex;

        &__text {
            flex-grow: 1;
            line-height: 3em;
            min-height: 3em;
            position: relative;
            box-sizing: border-box;
        }

        &__title {
            display: block;
            padding: 0 10px;
        }

        &__edit {
            padding: 0 10px;
            line-height: 3em;
            box-sizing: border-box;
            width: 100%;
            font-size: inherit;
            border: none;
            background: rgba(204, 168, 115, 0.11);
        }

        &__duration {
            margin: 0 .5em;
            line-height: 3em;
            display: inline-block;
            color: gainsboro;
        }

        &__actions {
            line-height: 3em;
        }
    }

</style>
