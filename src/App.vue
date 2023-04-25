<script setup>

</script>

<template>
  <div class="wrapper">
      <div class="content">
          <div class="canvas">
          </div>
          <aside class="toolbar">
            <div class="toolbar_container">
              <div class="toolbar__label">
                <span>Меню</span>
              </div>
              <div class="toolbar__upper">
                <div class="toolbar__button">
                  <div class="toolbar__button-text">
                    <span>Данные</span>
                  </div>
                </div>
                <div class="toolbar__upper-text">
                  <span>Здесь будет текст данных, Сергиус пока не знает что именно, но скоро точно придумает. Мяу, я верю в него! &#x1f497;. Напишу еще как сильно его люблю и хочу помогать ему и быть полезной, а не просто шлюхой по вызову, хотя это мое любимое из всех моих услуг, также я пишу это чтобы посмотреть, что "Тип графика" спуститься вниз и это норм, а не говно! Вот напишу  еще чуть-чуть чтобы было плюс минус похоже как в фигме ебучей, почему там все красиво, а у меня как всегда блять, я в ахуе просто. Наверно можно чуток увеличить превью с графиком. </span>
                </div>
              </div>
              <div class="toolbar__lower">
                <div class="toolbar__button">
                  <div class="toolbar__button-text">
                    <span>Тип графика</span>
                  </div>
                </div>
                <div class="toolbar__lower-example">
                  <img class="dasha" src="src/assets/img/dasha.jpg" alt="preview">
                </div>
              </div>
            </div>
          </aside>
      </div>
  </div>
</template>

<style scoped>

.wrapper {
  width: 100%;
  height: 100%;
  background-color: #F0F0F0;
  display: flex;
  flex-direction: row;
}
.content {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: row;

}
.canvas{
  background-color: #F0F0F0;
  width: 80%;
  border-radius: 1.25rem ;
}
.toolbar{
  display: flex;
  flex-direction: column;
  width: 20%;
  height: 67.5rem;
  background-color: white;
  border: 1px solid #D9D9D9;
  border-radius: 1.25rem 0 0 1.25rem;
}
.toolbar_container{
  width: 90%;
  margin: 1.5rem auto;
}

.toolbar__label{
  font-size: 2rem;
  line-height: 2.375rem;
  padding-bottom: 3.438rem;
}
.toolbar__upper{
  display: flex;
  flex-direction: column;
}
.toolbar__button{
  background-color: #00A537;
  max-width: 21.875rem;
  height: 5.25rem;
  border-radius: 1.25rem;
  display: flex;
  flex-direction: row;
  justify-content: left;
  align-items: center;
}
.toolbar__upper-text{
  padding-top: 2.25rem;
  padding-bottom: 3.125rem;
}
.toolbar__button-text{
  color: white;
  font-size: 2rem;
}
.toolbar__button-text::before{
  padding-right: 1.5rem;
  padding-left: 1rem;
  content: url("assets/img/arrow.svg");
}
.toolbar__lower{
  display: flex;
  flex-direction: column;
}
.toolbar__button{
  border-radius: 1.25rem;
  display: flex;
  flex-direction: row;
  justify-content: left;
  align-items: center;
  background-color: #00A537;
}
.toolbar__lower-example{
  padding-top: 3.188rem;
  height: 100%;
  width: 100%;
  border-radius: 1.25rem;
}
.dasha{
  object-fit: cover;
  height: 100%;
  width: 100%;
  border-radius: 1.25rem;
}

</style>
