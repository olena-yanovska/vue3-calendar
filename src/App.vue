<script>
export default {
  data() {
    return {
      days: ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Нд'],
    };
  },
  methods: {
    prevMonth() {},

    nextMonth() {},
  }
};
</script>

<template>
  <div class="page">
      <table class="table table is-bordered table is-striped table is-hoverable">
      <thead >
        <tr class="is-selected">
          <td colspan="7">
            <div class="table-head">
              <p class="month-row"><button class="button is-rounded button-left" @click="prevMonth"></button></p>
            <p class="month-row">Березень 2023</p>
            
            <p class="month-row"><button class="button is-rounded button-right" @click="nextMonth"></button></p>
            </div>
          </td>
        </tr>

        <tr>
          <th v-for="(name, index) in days" :key="index">{{ name }}</th>
        </tr>
      </thead>

      <tbody>
        <tr>
          <td>1</td>
          <td>2</td>
          <td>3</td>
          <td>4</td>
          <td>5</td>
          <td>6</td>
          <td>7</td>
        </tr>
        <tr>
          <td>1</td>
          <td>2</td>
          <td>3</td>
          <td>4</td>
          <td>5</td>
          <td>6</td>
          <td>7</td>
        </tr>
        <tr>
          <td>1</td>
          <td>2</td>
          <td>3</td>
          <td>4</td>
          <td>5</td>
          <td>6</td>
          <td>7</td>
        </tr>
        <tr>
          <td>1</td>
          <td>2</td>
          <td>3</td>
          <td>4</td>
          <td>5</td>
          <td>6</td>
          <td>7</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>



<style>
.page {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  height: 100%;
}

.table-head {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.month-row {
  display: inline-block;
  padding-left: 10px;
  padding-right: 10px;
}

.button-left {
  background-image: url(./assets/images/arrow-left.svg);

  background-position: center;
  background-repeat: no-repeat;
}

.button-right {
  background-image: url(./assets/images/arrow-right.svg);

  background-position: center;
  background-repeat: no-repeat;
}
</style>
