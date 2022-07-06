<template>
  <div class="demo-container">
    <h1>Vue Business Hours</h1>
    <p>Vue component for setting business hours in an administration panel.</p>
    <a href="https://github.com/sbarry50/vue-business-hours">Github</a>
    <div class="demo-component">
      <h2>Business Hours</h2>
      <div class="teste">
        <business-hours
          :days="days"
          :localization="textos"
          name="holidayHours"
          type="select"
          :time-increment="60"
          color="#2563EB"
          :hourFormat24="true"
        ></business-hours>
      </div>
    </div>
  </div>
</template>

<script>
import demoDays from './data/demoDays';
import demoDaysSpanish from './data/demoDaysSpanish';
import demoHolidays from './data/demoHolidays';
import demoDaysErrors from './data/demoDaysErrors';
import demoSpanishLocalization from './data/demoSpanishLocalization';
import demoDaysEmit from './data/demoDaysEmit';

export default {
  data() {
    return {
      days: {
        monday: [
          {
            open: '0900',
            close: '1200',
            id: '1',
            isOpen: true
          },
          {
            open: '1300',
            close: '1800',
            id: '2',
            isOpen: false
          }
        ],
        tuesday: [
          {
            open: '0900',
            close: '1200',
            id: '3',
            isOpen: true
          },
          {
            open: '1300',
            close: '1800',
            id: '4',
            isOpen: false
          }
        ],
        wednesday: [
          {
            open: '0900',
            close: '1200',
            id: '5',
            isOpen: true
          },
          {
            open: '1300',
            close: '1800',
            id: '6',
            isOpen: false
          }
        ],
        thursday: [
          {
            open: '0900',
            close: '1200',
            id: '7',
            isOpen: true
          },
          {
            open: '1300',
            close: '1800',
            id: '8',
            isOpen: false
          }
        ],
        friday: [
          {
            open: '0900',
            close: '1200',
            id: '9',
            isOpen: true
          },
          {
            open: '1300',
            close: '1800',
            id: '10',
            isOpen: false
          }
        ],
        saturday: [
          {
            open: '0900',
            close: '1300',
            id: '11',
            isOpen: true
          }
        ],

        sunday: [
          {
            open: '',
            close: '',
            id: '12',
            isOpen: false
          }
        ]
      },
      demoDays: demoDays,
      demoDaysSpanish: demoDaysSpanish,
      demoHolidays: demoHolidays,
      demoDaysErrors: demoDaysErrors,
      demoSpanishLocalization: demoSpanishLocalization,
      demoDaysEmit: demoDaysEmit
    };
  },
  methods: {
    updatedHours: function(val) {
      // eslint-disable-next-line
      console.log(val);
    }
  }
};
</script>

<style scoped>
.teste {
  width: 400px;
  border: solid 1px black;
}
.demo-container {
  margin: 50px auto;
  width: 800px;
  font-family: -apple-system, Helvetica, Arial, sans-serif;
  color: #3d4852;
}

.demo-component {
  width: 660px;
  margin-bottom: 50px;
}

.demo-component.wide {
  width: 800px;
}
</style>
