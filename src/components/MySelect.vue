<template>
  <div class="mc-select-wrapper" v-custom-click-outside="toggleDropdown" :style="{height: height, width: width}">
    <div class="mc-select__selected" @click="showDropdown = !showDropdown" :style="{height: height, width: width}">
      <input type="text" class="selected" :placeholder="placeholder" readonly :value="getSelected">
    </div>
    <div 
      class="mc-select-dropdown" 
      v-if="showDropdown" 
      :style="{'max-height': dropdownHeigth, 'top': height}">
      <div class="search-block" :style="{height: optionHeight, 'min-height': optionHeight}" v-if="search">
        <input type="text" class="search-input" placeholder="Search" v-model="searchText">
      </div>
        <div 
          @click="check(option)"
          :style="{height: optionHeight, 'min-height': optionHeight}"
          class="mc-select-dropdown__option" 
          :class="{checked: value.some(item => JSON.stringify(item) === JSON.stringify(option)) }"
          v-for="option in getOptions" :key="option[track_by]">
          <span class="option-name" v-html="option[label]"></span>
        </div>
      </div>
  </div>
</template>

<script>
  export default {
    props: {
      height: {type: String, default: '48px'},
      width: {type: String, default: '320px'},
      dropdownHeigth: {type: String, default: '200px'},
      optionHeight: {type: String, default: '48px'},
      placeholder: {type: String, default: 'Select option'},
      options: {type: Array, required: true},
      label: {type: String, default: 'name'},
      track_by: {type: String, default: 'key'},
      value: {type: Array, default: []},
      search: {type: Boolean, default: true}
    },
    data: () => ({
      showDropdown: false,
      searchText: ''
    }),
    methods: {
      toggleDropdown(){
        if(this.showDropdown) this.showDropdown = false
      },
      check(option){
        if(this.value.some(item => JSON.stringify(item) === JSON.stringify(option))){
          let newValue = this.value.filter(val => val[this.track_by] != option[this.track_by])
          this.$emit('input', newValue);
        }else{
          let newValue = this.value
          newValue.push(option)
          this.$emit('input', newValue);
        }
      }
    },
    directives: {
      'custom-click-outside': {
        bind(el, binding) {
          el.clickOutsideEvent = function (event) {
            if (!(el === event.target || el.contains(event.target))) {
              binding.value(event);
            }
          };
          document.addEventListener('click', el.clickOutsideEvent);
        },
        unbind(el) {
          document.removeEventListener('click', el.clickOutsideEvent);
        },
      },
    },
    computed: {
      getSelected: {
        get(){
          return this.value.map(val => val[this.label]).join(', ')
        },
      },
      getOptions(){
        if(this.search){
          if(this.searchText == ''){
            return this.options
          }else{
            let arr = this.options.map((item) => {
              if(item[this.label].toLowerCase().indexOf(this.searchText.toLowerCase()) >= 0){
                // item[this.label] = item[this.label].replace(this.searchText, `<strong>${this.searchText}</strong>`)
                // console.log('item::: ', item[this.label]);
                return item
              }else{
                return -1
              }
            })
            return arr.filter(item => item != -1)
          }
          
        }else{
          return this.options
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
.mc-select-wrapper{
  display: flex;
  align-items: center;
  flex-direction: column;
  position: relative;
  *{
    box-sizing: border-box;
  }
  .mc-select__selected{
    border: 1px solid #dcdcdc;
    width: 100%;
    border-radius: 8px;
    display: flex;
    align-items: center;
    padding: 0 16px;
    .selected{
      max-width: calc(100% - 16px);
      width: 100%;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      border: none !important;
      background: none !important;
      outline: none !important;
      pointer-events: none;
    }
    &:hover{
      cursor: pointer;
    }
  }
  .mc-select-dropdown{
    display: flex;
    flex-direction: column;
    // height: 480px;
    width: 100%;
    box-shadow: 0 0 1px #000000;
    position: absolute;
    background: #ffffff;
    overflow-y: auto;
    &::-webkit-scrollbar {
      width: 5px;
      background-color: #dcdcdc;
      position: absolute;
    }
    &::-webkit-scrollbar-thumb {
      background-color: rgb(182, 180, 180);
      border-radius: 9px;
    }

    &::-webkit-scrollbar-thumb:hover {
      background-color: black;
    }
    .mc-select-dropdown__option{
      display: flex;
      align-items: center;
      padding-left: 16px;
      strong{
        font-weight: bold;
      }
      &:hover{
        cursor: pointer;
        background: #dcdcdc;
      }
      &.checked{
        background: lightblue;
      }
    }
    .search-block{
      position: sticky;
      top: 0;
      background: #ffffff;
      display: flex;
      align-items: center;
      padding: 8px 16px;
      .search-input{
        border: 1px solid #dcdcdc;
        border-radius: 8px;
        width: 100%;
        outline: none;
        padding: 0 12px;
        height: 100%;
      }
    }
  }
}
</style>