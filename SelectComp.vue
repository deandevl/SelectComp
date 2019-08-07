<template>
  <div>
    <div class="selectComp" tabindex="0" v-on:blur="blur_it">
      <div class="selectComp_headingBox" v-if="heading">{{heading}}</div>
      <section :class="select_box_class">
        <div :class="is_open ? 'selectComp_arrowIcon__open' : 'selectComp_arrowIcon__closed'" v-on:click="arrow_click"></div>
        <div class="selectComp_selectBox">{{current_value}}</div>
      </section>
      <section class="selectComp_itemsSec" v-show="items">
        <div class="selectComp_itemsPanel" :style="panel_style">
          <div
              class="selectComp_itemBox"
              v-for="(item,index) in items"
              :key="item"
              v-on:click="item_click(item)">{{item}}
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'SelectComp',
    data(){
      return {
        current_value: this.select_value,
        is_open: false,
        panel_style: 'max-width: 0;max-height: 0; transition: all 3s; opacity: 0;',
        select_box_class: 'selectComp_selectBoxSec'
      }
    },
    props: {
      items: {
        type: Array,
        default: null
      },
      select_value: {
        type: String,
        default: null
      },
      heading: {
        type: String,
        default: null
      },
      placeholder: {
        type: String,
        default: () => {
          return null;
        }
      },
      drop_panel_height: {
        type: String,
        default: '6.5rem'
      },
      blur_panel: {
        type: Boolean,
        default: true
      },
      single_border: {
        type: Boolean,
        default: false
      },
      css_variables: {
        type: Object,
        default: () => {
          return null;
        }
      }
    },
    watch: {
      select_value: function(){
        if(this.current_value !== this.select_value){
          if(this.select_value === null && this.placeholder !== null){
            this.current_value = this.placeholder;
            this.$emit('select_comp_value_changed',null);
          }else{
            const idx = this.items.indexOf(this.select_value);
            if(idx !== -1){
              this.current_value = this.select_value;
              this.$emit('select_comp_value_changed',this.current_value);
            }
          }
        }
      }
    },
    methods: {
      blur_it: function(){
        if(this.is_open && this.blur_panel){
          this.is_open = false;
          this.panel_style = 'max-width: 0;max-height: 0;transition: all 3s; opacity: 0;';
        }
      },
      arrow_click: function(){
        this.toggle_panel_state();
      },
      item_click: function(item){
        this.current_value = item;
        this.$emit('select_comp_value_changed',item);
        this.toggle_panel_state();
      },
      toggle_panel_state: function(){
        if(this.is_open){
          this.is_open = false;
          this.panel_style = 'max-width: 0;max-height: 0;transition: all 3s; opacity: 0;';
        }else {
          this.is_open = true;
          this.panel_style = 'max-width: 100%;max-height: ' + this.drop_panel_height + '; transition: all 3s; opacity: 1.0;';
        }
      }
    },
    mounted(){
      if(this.select_value === null && this.placeholder !== null){
        this.current_value = this.placeholder;
      }else{
        const idx = this.items.indexOf(this.select_value);
        if(idx !== -1){
          this.current_value = this.select_value;
        }
      }

      if(this.single_border){
        this.select_box_class = 'selectComp_selectBoxSecBottomBorder';
      }

      if(this.css_variables !== null){
        for(let key of Object.keys(this.css_variables)){
          this.$el.style.setProperty(`--${key}`, this.css_variables[key]);
        }
      }
    }
  }
</script>

<style lang="less">
  :root {
    --select_comp_font_family: Verdana,serif;

    --select_comp_arrow_icon: '\21D3';
    --select_comp_arrow_color: black;

    --select_comp_width: 10rem;
    --select_comp_font_size: 1rem;
    --select_comp_color: darkgray;
    --select_comp_background: transparent;
    --select_comp_border_color: black;

    --select_comp_heading_font_size: 1rem;
    --select_comp_heading_color: black;
    --select_comp_heading_font_weight: bold;

    --select_comp_items_panel_color: black;
    --select_comp_items_panel_background: white;
    --select_comp_items_panel_border: 1px solid black;
    --select_comp_items_panel_position: static;
    --select_comp_items_panel_z_index: auto;

    --select_comp_item_font_size: .75rem;
    --select_comp_item_hover_box_shadow: 0 0 10px gray;
    --select_comp_item_hover_color: violet;
  }

  .selectComp {
    width: var(--select_comp_width);
    font-family: var(--select_comp_font_family);
    &:focus {
      outline: none;
    }

    &_headingBox {
      text-align: center;
      color: var(--select_comp_heading_color);
      font-size: var(--select_comp_heading_font_size);
      font-weight: var(--select_comp_heading_font_weight);
    }

    &_selectBoxSec {
      display: flex;
      flex-direction: row;
      color: var(--select_comp_color);
      background: var(--select_comp_background);
      font-size: var(--select_comp_font_size);
      border: 1px solid var(--select_comp_border_color);
    }

    &_selectBoxSecBottomBorder {
      display: flex;
      flex-direction: row;
      color: var(--select_comp_color);
      background: var(--select_comp_background);
      font-size: var(--select_comp_font_size);
      border: none;
      border-bottom: 1px solid var(--select_comp_border_color);
    }

    &_selectBox {
      padding: .2rem;
    }

    &_arrowIcon__open::before {
      content: var(--select_comp_arrow_icon);
      display: inline-block;
      color: var(--select_comp_arrow_color);
      transition: 600ms linear all;
      margin-right: .2rem;
      font-size: 1rem;
      cursor: pointer;
    }

    &_arrowIcon__closed::before {
      content: var(--select_comp_arrow_icon);
      display: inline-block;
      color: var(--select_comp_arrow_color);
      transform: rotate(-90deg);
      transition: 600ms linear all;
      margin-right: .2rem;
      font-size: 1rem;
      cursor: pointer;
    }

    &_itemsSec {
      position: relative;
    }

    &_itemsPanel {
      color: var(--select_comp_items_panel_color);
      background: var(--select_comp_items_panel_background);
      border: var(--select_comp_items_panel_border);
      position: var(--select_comp_items_panel_position);
      z-index: var(--select_comp_items_panel_z_index);
      border-bottom-left-radius: 4px;
      border-bottom-right-radius: 4px;
      padding: .5rem;
      width: max-content;
      transition: all 2s ease-in-out;
      overflow: auto;

      &::-webkit-scrollbar-track {
        background-color: #F5F5F5;
        border-radius: 10px;
        -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
      }
      &::-webkit-scrollbar {
        background-color: transparent;
        width: 12px;
        height: 12px;
      }
      &::-webkit-scrollbar-thumb {
        background-color: #D62929;
        border-radius: 10px;
        -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, .3);
      }
    }

    &_itemBox {
      cursor: pointer;
      margin: .3rem;
      font-size: var(--select_comp_item_font_size);

      &:hover {
        color: var(--select_comp_item_hover_color);
        box-shadow: var(--select_comp_item_hover_box_shadow);
      }
    }
  }
</style>