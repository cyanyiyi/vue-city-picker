<template>
    <div class="city-picker-container">
      <div class="city-picker-input" :class="classes" :style="{color: value ? '#2c3e50' : '#9b9b9b'}" @tap="e_openPick">
        {{ value ? value : placeholder }}
      </div>
      <div class="city-popup-wrap" v-if="showPopup">
        <div class="city-popup-content">
          <div class="city-popup-title">
            请选择地址
            <div class="city-icon-item" @tap="e_closePick">
              <span class="city-close">x</span>
            </div>
          </div>
          <div class="city-popup-select">
            <span class="city-show" @tap="e_selectProvince" :class="{'city-active': !province}">{{province ? province : '请选择'}}</span>
            <span class="city-show" v-if="province" @tap="e_selectCity" :class="{'city-active': !city}">{{city ? city : '请选择'}}</span>
            <span class="city-show" v-if="province && city" @tap="e_selectDistrict" :class="{'city-active': !district}">{{district ? district : '请选择'}}</span>
          </div>
          <div class="city-popup-pick">
            <span class="city-pick-item" v-if="showProvince" v-for="item in dataList" :key="item.value" :class="{'pick-active': item.text === province}" @tap="e_pickProvince(item.value, item.text)">{{ item.text }}</span>
            <span class="city-pick-item" v-if="showCity" v-for="item in cityList" :key="item.value" :class="{'pick-active': item.text === city}" @tap="e_pickCity(item.value, item.text)">{{ item.text }}</span>
            <span class="city-pick-item" v-if="showDistrict" v-for="item in districtList" :key="item.value" :class="{'pick-active': item.text === district}" @tap="e_pickDistrict(item.value, item.text)">{{ item.text }}</span>
          </div>
          </div>
      </div>
    </div>
</template>
<script>
import cityData from './city.data-3.js';
export default {
  components: {},
  props: {
    value: String,
    classes: String,
    placeholder: {
      type: String,
      default: '请选择'
    }
  },
  data () {
    return {
      dataList: cityData,
      showPopup: false,
      showProvince: true,
      showCity: false,
      showDistrict: false,
      showCityList: false,
      showDistrictList: false,
      province: '',
      city: '',
      district: ''
    }
  },
  computed: {
    cityList () {
      let tmpList = []
      this.dataList.map((v, k) => {
        if (v.text === this.province) {
          tmpList = v.children;
        }
      });
      return tmpList
    },
    districtList () {
      let tmpList = []
      this.cityList.map((v, k) => {
        if (v.text === this.city) {
          tmpList = v.children;
        }
      });
      return tmpList
    }
  },
  watch: {
    value: function (val, oldVal) {
      console.log(val)
      this.$emit('address-input', val);
    }
  },
  methods: {
    e_openPick () {
      this.showPopup = true;
    },
    e_closePick () {
      this.showPopup = false;
    },
    e_selectProvince () {
      this.showProvince = true;
      this.showCity = false;
      this.showDistrict = false;
    },
    e_selectCity () {
      this.showProvince = false;
      this.showCity = true;
      this.showDistrict = false;
    },
    e_selectDistrict () {
      this.showProvince = false;
      this.showCity = false;
      this.showDistrict = true;
    },
    e_pickProvince (value, text) {
      this.province = text;
      this.city = '';
      this.district = '';
      this.showProvince = false;
      this.showCity = true;
      this.showDistrict = false;
    },
    e_pickCity (value, text) {
      this.city = text;
      this.district = '';
      this.showProvince = false;
      this.showCity = false;
      this.showDistrict = true;
    },
    e_pickDistrict (value, text) {
      this.district = text;
      this.value = this.province + ' ' + this.city + ' ' + this.district;
      setTimeout(() => {
        this.showPopup = false;
        this.showProvince = true;
        this.showCity = false;
        this.showDistrict = false;
      }, 500)
    }
  },
  onLoad () {
    console.log('onload')
  }
}
</script>
<style>
.city-picker-container {
  width: 100%;
  height: 80px;
  display: inline-block;
  /* border: 1px solid #ccc; */
}
.city-picker-container .city-picker-input {
  width: 100%;
  height: 80px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.city-popup-wrap {
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.4);
  position: fixed;
  left: 0;
  bottom: 0;
  z-index: 99;
}

.city-popup-wrap .city-popup-content {
  width: 100%;
  height: 820px;
  position: absolute;
  left: 0;
  bottom: 0;
  z-index: 999;
  background: #fff;
}

.city-popup-wrap .city-popup-title {
  height: 60px;
  line-height: 60px;
  text-align: center;
  position: relative;
}

.city-popup-wrap .city-popup-title .city-icon-item {
  display: inline-block;
  width: 60px;
  height: 60px;
  line-height: 60px;
  text-align: center;
  position: absolute;
  top: 0px;
  right: 10px;
}

.city-popup-wrap .city-popup-select {
  height: 60px;
  line-height: 60px;
  padding: 0 10px;
  border-bottom: 1px solid #ddd;
}
.city-popup-wrap .city-popup-select .city-show {
  display: inline-block;
  padding: 0 15px;
  color: #2c3e50;
}
.city-popup-wrap .city-popup-select .city-show.city-active {
  color: #f56c6c;
}
.city-popup-wrap .city-popup-pick {
  height: 660px;
  padding: 20px 30px;
  overflow-y: scroll;
}
.city-popup-wrap .city-popup-pick .city-pick-item {
  height: 60px;
  line-height: 60px;
  color: #2c3e50;
}
.city-popup-wrap .city-popup-pick .city-pick-item.pick-active {
  color: #f56c6c;
}
</style>
