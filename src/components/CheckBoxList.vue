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
            <div class="checkbox-label">
                <label>
                    <input class="checkbox" :value="option" type="checkbox" v-model="checkedOptions">
                    {{ option }}
                </label>
            </div>
            <button @click="closeButtonHandler(option)" class="delete-button">
                <svg class="delete-button__icon" width="25" height="24" viewBox="0 0 25 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M23 2L2 22" :stroke="checkThisOption(option)? 'white' : 'black'" stroke-width="3" stroke-linecap="round"/>
                    <path d="M23 22L2 2" :stroke="checkThisOption(option)? 'white' : 'black'" stroke-width="3" stroke-linecap="round"/>
                </svg>
            </button>
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
    border-radius: 1rem;
    background-color: #fff;
    display: flex;
    justify-content: space-between;
    flex-direction: row;
    transition: box-shadow;

}

.displayed {
    color: #fff;
    background-color: #00A537;
    box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}

.checkbox {
    display: none;
}

.checkbox-label {

    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    color: inherit;
}
.delete-button {
    max-width: 46px;
    background-color: inherit;
    border: none;
    border-radius: 1rem;
}
.delete-button__icon {
    width: 34px;
    height: 34px;
}
</style>