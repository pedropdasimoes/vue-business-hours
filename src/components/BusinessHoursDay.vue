<template>
  <div is="transition-group" name="fade">
    <div v-for="({ open, close, id, isOpen }, index) in hours" :key="id">
      <div class="flex-table" role="rowgroup">
        <div v-if="showDay(index) && day != 'monday'" class="hr"></div>
        <div class="separation">
          <div class="flex-row day" role="cell">
            <div v-if="showDay(index)">{{ localization.days[day] }}</div>
          </div>
          <div class="flex-row toggle" role="cell">
            <ToggleButton
              v-if="showDay(index)"
              @change="
                toggleOpen();
                resetHours();
                runValidations();
              "
              :value="anyOpen"
              :sync="true"
              :labels="{
                checked: localization.switchOpen,
                unchecked: localization.switchClosed
              }"
              :color="color"
              :width="switchWidth"
              :height="25"
              :font-size="12"
              style="margin-bottom:10px;"
            />
          </div>
        </div>

        <div class="separation">
          <transition name="fade">
            <div
              class="flex-row hours open"
              role="cell"
              v-visible="isOpenToday"
            >
              <BusinessHoursSelect
                v-if="type === 'select'"
                :name="name"
                :input-num="inputNum('open', index)"
                :total-inputs="totalInputs"
                :day="day"
                :hours="hours"
                :time-increment="timeIncrement"
                :index="index"
                :selected-time="open"
                :localization="localization"
                :hour-format24="hourFormat24"
                @input-change="onChangeEventHandler('open', index, $event)"
              ></BusinessHoursSelect>
              <BusinessHoursDatalist
                v-if="type === 'datalist'"
                :name="name"
                :input-num="inputNum('open', index)"
                :total-inputs="totalInputs"
                :day="day"
                :hours="hours"
                :time-increment="timeIncrement"
                :index="index"
                :selected-time="open"
                :any-error="anyError(validations[index].open)"
                :localization="localization"
                :hour-format24="hourFormat24"
                @input-change="onChangeEventHandler('open', index, $event)"
              ></BusinessHoursDatalist>
            </div>
          </transition>
          <transition name="fade">
            <div class="flex-row dash" role="cell" v-visible="isOpenToday">
              -
            </div>
          </transition>
          <transition name="fade">
            <div
              class="flex-row hours close"
              role="cell"
              v-visible="isOpenToday"
            >
              <BusinessHoursSelect
                v-if="type === 'select'"
                :name="name"
                :input-num="inputNum('close', index)"
                :total-inputs="totalInputs"
                :day="day"
                :hours="hours"
                :time-increment="timeIncrement"
                :index="index"
                :selected-time="close"
                :localization="localization"
                :hour-format24="hourFormat24"
                @input-change="onChangeEventHandler('close', index, $event)"
              ></BusinessHoursSelect>
              <BusinessHoursDatalist
                v-if="type === 'datalist'"
                :name="name"
                :input-num="inputNum('close', index)"
                :total-inputs="totalInputs"
                :day="day"
                :hours="hours"
                :time-increment="timeIncrement"
                :index="index"
                :any-error="anyError(validations[index].close)"
                :updated-validations="validations[index].close"
                :selected-time="close"
                :hour-format24="hourFormat24"
                :localization="localization"
                @input-change="onChangeEventHandler('close', index, $event)"
              ></BusinessHoursDatalist>
            </div>
          </transition>
          <div class="flex-row remove" role="cell" v-visible="isOpenToday">
            <button
              type="button"
              class="font-awesome-button"
              v-if="showRemoveButton()"
              @click="removeRow(index)"
            >
              <FontAwesomeIcon icon="times" class="fa-sm" />
            </button>
          </div>
        </div>
        <div class="separation">
          <div class="flex-row add" role="cell" v-visible="isOpenToday">
            <button
              type="button"
              :style="{ color: color }"
              class="add-hours"
              v-if="showAddButton(index)"
              @click="addRow()"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink"
                viewBox="0 0 20 20"
                width="20px"
                height="20px"
              >
                <defs>
                  <linearGradient
                    id="linear0"
                    gradientUnits="userSpaceOnUse"
                    x1="35.324501"
                    y1="35.324501"
                    x2="136.675507"
                    y2="136.675507"
                    gradientTransform="matrix(0.116279,0,0,0.116279,0,0)"
                  >
                    <stop
                      offset="0"
                      style="stop-color:rgb(59.607846%,71.764708%,98.823529%);stop-opacity:1;"
                    />
                    <stop
                      offset="1"
                      style="stop-color:rgb(28.627452%,43.921569%,78.039217%);stop-opacity:1;"
                    />
                  </linearGradient>
                  <filter
                    id="alpha"
                    filterUnits="objectBoundingBox"
                    x="0%"
                    y="0%"
                    width="100%"
                    height="100%"
                  >
                    <feColorMatrix
                      type="matrix"
                      in="SourceGraphic"
                      values="0 0 0 0 1 0 0 0 0 1 0 0 0 0 1 0 0 0 1 0"
                    />
                  </filter>
                  <mask id="mask0">
                    <g filter="url(#alpha)">
                      <rect
                        x="0"
                        y="0"
                        width="20"
                        height="20"
                        style="fill:rgb(0%,0%,0%);fill-opacity:0.05;stroke:none;"
                      />
                    </g>
                  </mask>
                  <image
                    id="image55213933"
                    width="20"
                    height="20"
                    xlink:href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAABmJLR0QA/wD/AP+gvaeTAAAAc0lEQVQ4jWNgGAVDEsgxMDC8g2I5QoqZiDBQiYGBQRCKlahhIEmABYe4PAMDgyqUrY8kboyk5zYDA8NDYi16x8DA8J8AfotNI928bMiA6uUeKLuEgYHhIpR9i1xLHRgQ3nQgpJhuXkYG9xggkQRjj4KhBgDCZhckLuy/jwAAAABJRU5ErkJggg=="
                  />
                  <mask id="mask1">
                    <g filter="url(#alpha)">
                      <rect
                        x="0"
                        y="0"
                        width="20"
                        height="20"
                        style="fill:rgb(0%,0%,0%);fill-opacity:0.07;stroke:none;"
                      />
                    </g>
                  </mask>
                  <image
                    id="image55213936"
                    width="20"
                    height="20"
                    xlink:href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAABmJLR0QA/wD/AP+gvaeTAAAAbUlEQVQ4je2RQQqAMAwExyoeLIgP8P8fqP7IF3jQk+glYBALFXKw0IGFZSEJS6CQJQ6YRM5ioQdOkU+5bkoTyWugF9+pfABa8StwpB4K3DVjCm+D5pWrSP6svIgfgU38p8qaf35ZswOz8oXcuABItxfFXfFb6AAAAABJRU5ErkJggg=="
                  />
                  <linearGradient
                    id="linear1"
                    gradientUnits="userSpaceOnUse"
                    x1="78.833328"
                    y1="86"
                    x2="93.166672"
                    y2="86"
                    gradientTransform="matrix(0.116279,0,0,0.116279,0,0)"
                  >
                    <stop
                      offset="0.824"
                      style="stop-color:rgb(6.27451%,15.294118%,34.117648%);stop-opacity:1;"
                    />
                    <stop
                      offset="0.931"
                      style="stop-color:rgb(6.27451%,14.901961%,33.725491%);stop-opacity:1;"
                    />
                    <stop
                      offset="1"
                      style="stop-color:rgb(5.882353%,13.333334%,29.411766%);stop-opacity:1;"
                    />
                  </linearGradient>
                  <linearGradient
                    id="linear2"
                    gradientUnits="userSpaceOnUse"
                    x1="46.583328"
                    y1="86"
                    x2="125.416672"
                    y2="86"
                    gradientTransform="matrix(0.116279,0,0,0.116279,0,0)"
                  >
                    <stop
                      offset="0.824"
                      style="stop-color:rgb(6.27451%,15.294118%,34.117648%);stop-opacity:1;"
                    />
                    <stop
                      offset="0.931"
                      style="stop-color:rgb(6.27451%,14.901961%,33.725491%);stop-opacity:1;"
                    />
                    <stop
                      offset="1"
                      style="stop-color:rgb(5.882353%,13.333334%,29.411766%);stop-opacity:1;"
                    />
                  </linearGradient>
                </defs>
                <g id="surface55213929">
                  <path
                    style=" stroke:none;fill-rule:nonzero;fill:url(#linear0);"
                    d="M 18.332031 10 C 18.332031 14.601562 14.601562 18.332031 10 18.332031 C 5.398438 18.332031 1.667969 14.601562 1.667969 10 C 1.667969 5.398438 5.398438 1.667969 10 1.667969 C 14.601562 1.667969 18.332031 5.398438 18.332031 10 Z M 18.332031 10 "
                  />
                  <use xlink:href="#image55213933" mask="url(#mask0)" />
                  <use xlink:href="#image55213936" mask="url(#mask1)" />
                  <path
                    style=" stroke:none;fill-rule:nonzero;fill:url(#linear1);"
                    d="M 9.582031 5.417969 L 10.417969 5.417969 C 10.648438 5.417969 10.832031 5.601562 10.832031 5.832031 L 10.832031 14.167969 C 10.832031 14.398438 10.648438 14.582031 10.417969 14.582031 L 9.582031 14.582031 C 9.351562 14.582031 9.167969 14.398438 9.167969 14.167969 L 9.167969 5.832031 C 9.167969 5.601562 9.351562 5.417969 9.582031 5.417969 Z M 9.582031 5.417969 "
                  />
                  <path
                    style=" stroke:none;fill-rule:nonzero;fill:url(#linear2);"
                    d="M 14.582031 9.582031 L 14.582031 10.417969 C 14.582031 10.648438 14.398438 10.832031 14.167969 10.832031 L 5.832031 10.832031 C 5.601562 10.832031 5.417969 10.648438 5.417969 10.417969 L 5.417969 9.582031 C 5.417969 9.351562 5.601562 9.167969 5.832031 9.167969 L 14.167969 9.167969 C 14.398438 9.167969 14.582031 9.351562 14.582031 9.582031 Z M 14.582031 9.582031 "
                  />
                </g>
              </svg>
            </button>
          </div>
        </div>
      </div>
      <ul class="time-errors" v-if="validations[index].anyErrors">
        <li
          v-for="{ whichTime, error } in activeErrors(index)"
          :key="whichTime + '.' + error"
        >
          {{ errorMessage(whichTime, error) }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import BusinessHoursSelect from './BusinessHoursSelect.vue';
import BusinessHoursDatalist from './BusinessHoursDatalist.vue';
import { ToggleButton } from 'vue-js-toggle-button';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { helperMixin } from '../mixins/helperMixin';
import { validationMixin } from '../mixins/validationMixin';
import uniqid from 'uniqid';
export default {
  name: 'BusinessHoursDay',
  components: {
    BusinessHoursSelect,
    BusinessHoursDatalist,
    ToggleButton,
    FontAwesomeIcon
  },
  mixins: [helperMixin, validationMixin],
  props: {
    day: {
      type: String,
      required: true
    },
    hours: {
      type: Array,
      required: true
    },
    name: {
      type: String,
      required: true
    },
    timeIncrement: {
      type: Number,
      required: true
    },
    type: {
      type: String,
      required: true
    },
    color: {
      type: String,
      required: true
    },
    localization: {
      type: Object
    },
    switchWidth: {
      type: Number
    },
    hourFormat24: {
      type: Boolean
    }
  },
  computed: {
    totalInputs: function() {
      return this.hours.length * 2;
    },
    isOpenToday: function() {
      return this.hours[0].isOpen;
    },
    anyOpen: function() {
      return this.hours.some(hour => {
        return hour.isOpen === true;
      });
    }
  },
  directives: {
    visible: function(el, binding) {
      el.style.visibility = binding.value ? 'visible' : 'hidden';
    }
  },
  methods: {
    onChangeEventHandler: function(whichTime, index, value) {
      value = this.backendInputFormat(value);

      if (value == '24hrs') {
        this.hours.splice(1);
        this.hours[0].open = this.hours[0].close = value;
        this.runValidations();
        this.updateHours();
        return;
      }

      if (
        (this.hours[index].open == '24hrs' ||
          this.hours[index].close == '24hrs') &&
        value == ''
      ) {
        this.hours[index].open = this.hours[index].close = value;
        this.runValidations();
        this.updateHours();
        return;
      }

      if (
        !this.onlyOneRow(this.hours) &&
        value === '' &&
        ((whichTime === 'open' && this.hours[index].close === '') ||
          (whichTime === 'close' && this.hours[index].open === ''))
      ) {
        this.removeRow(index);
        this.runValidations();
        this.updateHours();
        return;
      }

      this.hours[index][whichTime] = value;
      this.runValidations();
      this.updateHours();
    },
    inputNum: function(whichTime, index) {
      if (whichTime === 'open') {
        return index * 2 + 1;
      } else if (whichTime === 'close') {
        return index * 2 + 2;
      }
    },
    toggleOpen: function() {
      this.hours[0].isOpen = this.hours[0].isOpen ? false : true;
    },
    resetHours: function() {
      this.hours.splice(1);
      this.hours[0].open = this.hours[0].close = '';
      this.updateHours();
    },
    addRow: function() {
      this.hours.push({
        id: uniqid(),
        open: '',
        close: '',
        isOpen: true
      });
      this.runValidations();
      this.updateHours();
    },
    removeRow: function(index) {
      this.hours.splice(index, 1);
      this.runValidations();
      this.updateHours();
    },
    showDay: function(index) {
      return index > 0 ? false : true;
    },
    showRemoveButton: function() {
      return this.hours.length > 1 ? true : false;
    },
    showAddButton: function(index) {
      return this.hours.length === index + 1 &&
        this.hours[index].open !== '' &&
        this.hours[index].close !== '' &&
        this.hours[index].open !== '24hrs' &&
        this.hours[index].close !== '24hrs' &&
        !(
          this.type === 'select' &&
          this.timeIncrement === 15 &&
          this.hours[index].close === '2345'
        ) &&
        !(
          this.type === 'select' &&
          this.timeIncrement === 30 &&
          this.hours[index].close === '2330'
        ) &&
        !(
          this.type === 'select' &&
          this.timeIncrement === 60 &&
          this.hours[index].close === '2300'
        ) &&
        this.hours[index].close !== '2400' &&
        this.validations[index].anyErrors === false
        ? true
        : false;
    },
    updateHours: function() {
      const updatedHours = { [this.day]: this.hours };
      this.$emit('hours-change', updatedHours);
    }
  }
};
</script>

<style lang="scss" scoped>
.hr {
  width: 80%;
  height: 1px;
  margin-bottom: 25px;
  background-color: #2563eb;
  opacity: 35%;
}
.separation {
  display: flex;
}
.flex-table {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0.75em 0;
  height: fit-content;
  justify-content: center;
  width: 100%;
}

.flex-row {
  width: 20%;
}

.flex-row /deep/ input,
.flex-row /deep/ select {
  margin: 1px;
  padding: 3px 5px;
  width: 70px;
  height: 28px;
  font-size: 14px;
  line-height: 28px;
  vertical-align: middle;
  border: 1px solid #d5d5d5;
  box-sizing: border-box;
}

.flex-row.day {
  width: 90px;
}

.flex-row.hours {
  width: 70px;
}

.flex-row.dash {
  margin: 0 4px;
  width: 4px;
}

.row-container {
  flex-direction: column;
}

.row {
  flex-direction: row;
}

.remove {
  display: flex;
  justify-content: center;
  width: 50px;
}

label.vue-js-switch {
  margin-bottom: 0;
}

button.add-hours,
button.font-awesome-button {
  height: 30px;
  background-color: transparent;
  border-color: transparent;
  border-style: none;
  border-width: 0;
  padding: 0;
  cursor: pointer;
}

button.add-hours:focus,
button.font-awesome-button:focus {
  outline: none;
}

button.font-awesome-button {
  width: 30px;
  font-size: 24px;
}

button.add-hours {
  font-size: 14px;
  font-weight: bold;
}

.fa-times {
  color: #3d4852;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter,
.fade-leave-to
  /* .component-fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

.time-errors {
  margin: 0;
  padding: 0;
  font-size: 12px;
  color: #e3342f;
  list-style: none;
}

.time-errors li {
  margin-bottom: 6px;
}
</style>
