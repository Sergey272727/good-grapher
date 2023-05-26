<script>
export default {
    emits: {
        'change-option': value => typeof value === "string"
    },
    props: {
        options: {
            type: Array,
            required: true,
            default: [
                'option 1',
                'option 2',
                'option 3'
            ]
        },
        elementName: {
            type: String,
            required: false,
            default: 'Option'
        }
    },
    data() {
        return {
            isOpen: false,
            selectedOption: '',
            hoveredOption: null
        };
    },
    methods: {
        toggleDropdown() {
            this.isOpen = !this.isOpen;
        },
        changeGraphTypeEmitter() {
            this.$emit('change-option', this.selectedOption);
        }
    }
};
</script>
<template>
    <div class="dropdown"
         ref="dropdown"
    >
        <svg @click="toggleDropdown" class="dropdown-icon" :class="{ 'is-open': isOpen }" width="17" height="30"
             viewBox="0 0 17 30" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M16.4142 16.4142C17.1953 15.6332 17.1953 14.3668 16.4142 13.5858L3.68629 0.857864C2.90524 0.0768156 1.63891 0.0768156 0.857864 0.857864C0.0768156 1.63891 0.0768156 2.90524 0.857864 3.68629L12.1716 15L0.857864 26.3137C0.0768156 27.0948 0.0768156 28.3611 0.857864 29.1421C1.63891 29.9232 2.90524 29.9232 3.68629 29.1421L16.4142 16.4142ZM14 17H15V13H14V17Z"
                  fill="white"/>
        </svg>
        <select
                @focusout="isOpen = false;"
                ref="dropDownSelect"
                :class="{'is-open': isOpen}"
                v-model="selectedOption"
                class="dropdown-select"
                @click="toggleDropdown"
                @change='changeGraphTypeEmitter'
        >
            <option value="" selected disabled hidden> {{ elementName }}</option>
            <option v-for="(option, index) in options"
                    :key="index"
                    :value="option.key"
                    :class="{ 'hover': hoveredOption === index }"
                    @mouseenter="hoveredOption = index"
                    @mouseleave="hoveredOption = null"
            >
                {{ option.name }}
            </option>
        </select>
    </div>
</template>
<style scoped>
.dropdown {
    display: flex;
    flex-direction: row;
    align-items: center;
    align-content: center;
}

.dropdown-select {
    appearance: none;
    background-color: #00A537;
    border: none;
    border-radius: 20px;
    color: #ffffff;
    cursor: pointer;

    padding-left: 57px;
    box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
    font-size: 1.75rem;
    width: 100%;
    max-width: 21.875rem;
    height: 5.25rem;
    transition: border-radius 0.1s linear;
}

.dropdown-select.is-open {
    border-radius: 20px 20px 0 0;
}

.dropdown-icon {
    fill: #333;
    position: absolute;
    top: 50%;
    left: 5%;
    z-index: 1;
    transition: transform 0.2s ease;
    transform: translateY(-50%);
}

.dropdown-icon.is-open {
    transform: translateY(-50%) rotate(90deg);
}
</style>
