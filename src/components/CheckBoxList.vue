<script>
export default {
    name: "CheckBoxList",
    emits: {
        'change-option': null,
        'delete-element': null,
    },
    props: {
        checkBoxData: {
            type: Array,
            required: true,
            default: ['option 1', 'option 2', 'option 3']
        }
    },
    data() {
        return {
            checkedOptions: []
        }
    },
    methods: {
        closeButtonHandler(elementToDelete) {
            this.$emit('delete-element', elementToDelete);
        },
        checkThisOption(option) {
            let res = false;
            this.checkedOptions.forEach((value) => {
                if (value === option) {
                    res = true;
                }
            });
            return res;
        }
    },
    watch: {
        checkedOptions() {
            this.$emit('change-option', this.checkedOptions);
        }
    }
}
</script>

<template>
    <ul class="checkbox-list">
        <li v-for="(option, index) in checkBoxData"
            :key="index"
            class="checkbox-list__item no-select"
            :class="{'displayed': checkThisOption(option)}"
        >
            <label>
                <input class="checkbox" :value="option" type="checkbox" v-model="checkedOptions">
                {{ option }}
            </label>
            <button @click="closeButtonHandler(option)" class="close-button">X</button>
        </li>
    </ul>
</template>

<style scoped>
.no-select {
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}
.checkbox-list {
    list-style-type: none;
    padding: 1rem;
    margin: 0;
    border-left: 1px solid black;
    border-right: 1px solid black;
    display: flex;
    flex-direction: column;
}

.checkbox-list__item {
    margin: 0.5rem 0;
    padding: 0.5rem;
    border-radius: 1.25rem;
    background-color: #fff;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

.displayed {
    color: #fff;
    background-color: #00A537;
}

.checkbox {
    display: none;
}

label {

}
</style>