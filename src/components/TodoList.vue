<template>
    <draggable class="dragArea list-group" :list="todos" :clone="clone" :group="{ name: 'people', pull: pullFunction }"
        @start="start">
        <li v-for="(item, index) in todos" :key="item.text">
            <label>
                <input :checked="completed" @change="toggle(item, index)" type="checkbox" />
                <span @click="toggle(item, index)">{{ item.text }}</span>
                <a href="#" class="remove" @click="remove(item.id)">remove</a>
            </label>
        </li>
    </draggable>
</template>

<script>
import draggable from "vuedraggable";

export default {
    name: 'HelloWorld',
    components: {
        draggable
    },
    props: {
        todos: {
            type: Array,
            required: true
        },
        completed: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            controlOnStart: true
        }
    },
    methods: {

        remove(id) {
            this.$emit('remove', id, this.completed)
        },
        pullFunction() {
            return this.controlOnStart ? "clone" : true;
        },
        start({ originalEvent }) {
            this.controlOnStart = originalEvent.ctrlKey;
        },
        clone(obj) {
            return { ...obj, id: new Date().getTime() };
        },
        toggle(item, index) {
            this.$emit('toggle', item, index)
        },



    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
