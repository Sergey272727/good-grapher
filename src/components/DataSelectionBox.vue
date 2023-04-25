<script>
export default {
    name: "DataSelectionBox"
}
</script>

<template>
    <div class="data-selection-box">
        <div class="data-selection-box__button">
            <div class="data-selection-box__button__text">Данные</div>
            <input type="file" class="file-input">
        </div>
        <div class="data-selection-box__text">

        </div>
    </div>
</template>



<style scoped>
.file-input {
    display: none;
}
.data-selection-box__button{
    display: flex;
    justify-content: space-between;
    width: 21.875rem;
    height: 5.25rem;
    color: #ffffff;
    background: #00A537;
    border-radius: 1.25rem;
}
.data-selection-box__button__text {
    font-family: Roboto,sans-serif;
    font-size: 2rem;
    padding: 0.938rem 1.25rem;
}
.data-selection-box__button__text::before {
    padding: 0.625rem;
    margin-right: 3.125rem;
    content: url(../assets/img/arrow.svg);
}
</style>