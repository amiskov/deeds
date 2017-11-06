<template>
    <li class="deed">
        <span class="deed__time">{{time}}</span>

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
    const dateOptions = {hour: '2-digit', minute: '2-digit'};
    export default {
        name: 'deed',
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
        computed: {
            time: function () {
                return new Date(this.item.date).toLocaleString('ru-RU', dateOptions)
            }
        },
        methods: {
            saveDeed(event) {
                this.isEditable = false
                this.$emit('save')
            },
            edit (param) {
                this.$emit('edit', param)
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

        &__time {
            line-height: 3em;
            margin: 0 .5em;
        }

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
