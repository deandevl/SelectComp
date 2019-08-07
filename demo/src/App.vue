<template>
  <div class="demo_container">
    <div class="buttons_box">
      <button v-on:click="add_friend">Add Friend 'Johnny'</button>
      <button v-on:click="del_friend">Delete Friend 'Pete'</button>
      <button v-on:click="set_travel_value">Set Travel to 'Cleveland'</button>
      <button v-on:click="set_travel_null">Set Travel to null</button>
      <button v-on:click="set_new_travels">Set New Travel List</button>
      <button v-on:click="show_current_values">Show Current Values (at console)</button>
    </div>

    <div class="selects_box">
      <select-comp
          heading="My Best Friend"
          placeholder="Select a friend"
          select_value="Jim"
          :items="items_friends"
          :blur_panel="blur_panel"
          :single_border="single_border_friends"
          :css_variables="css_variables_friends"
          v-on:select_comp_value_changed="value => {this.current_best_friend = value}">
      </select-comp>

      <select-comp
          heading="This Year's Travel Spot"
          placeholder="Select travel spot"
          :items="items_travels"
          :select_value="current_travel_spot"
          :css_variables="css_variables"
          v-on:select_comp_value_changed="value => {this.current_travel_spot = value}">
      </select-comp>
    </div>
    <section class="pbox">
      <p>
        rownwnn andlle  aljnd;3e; .akdnend  qnfdslns alne. andl;; irjlsek  sljdnel
        ewlnsl lsnkei s nelnsndl  slhelnskdn lslsn; a;ke;;;0ekd  lsnlsnel sllls
        2nwlenkf lsmksnel; slasll slenen ls nellllsn 5nrlnelwn lsnl  slnwnl  rien
        wqnelnwlen tltnylnr lsnenl wqnsna slns yktnu iims wemr eeenelsn ssysnwl snn
        rlrnelwn lsnen  rlnelwn alqnfkt ndlnwln eerlnw qnerndlsnl yytl alsn lkd5lr
        endsbx alnsnr uyntlrnl lln wndjf prpt wenrnf qwns duu tnrhdhh slnfldn wekr
      </p>
    </section>
  </div>
</template>

<script>
  import SelectComp from 'selectcomp';

  export default {
    name: 'App',
    data(){
      return {
        items_friends: [
          'Rick',
          'Nancy',
          'Jim',
          'Pete',
          'Harry',
          'Sally',
          'Jill',
          'Bob'
        ],

        items_travels: [
          'New York',
          'Boston',
          'Cleveland',
          'San Francisco',
          'Chicago',
          'Detroit',
          'St. Louis',
          'Toledo'
        ],
        current_best_friend: 'Jim',
        current_travel_spot: null,
        blur_panel: false,
        single_border_friends: true,
        css_variables_friends: {
          select_comp_width: '12.5rem',
          select_comp_border_color: 'green',
          select_comp_items_panel_background: 'lightgreen',
          select_comp_items_panel_position: 'absolute',
          select_comp_items_panel_z_index: '100'
        },
        css_variables: {
          select_comp_width: '14rem',
          select_comp_border_color: 'green'
        }
      };
    },
    components: {
      SelectComp
    },
    methods: {
      //parent to child -- add to friends list
      add_friend: function(){
        this.items_friends.push('Johnny');
      },
      //parent to child -- delete from friends list
      del_friend: function(){
        const idx = this.items_friends.indexOf('Pete');
        if(!(idx < 0)){
          this.items_friends.splice(idx, 1);
        }
      },
      //parent to child -- set value
      set_travel_value: function(){
        this.current_travel_spot = 'Cleveland';
      },
      //parent to child -- set value
      set_travel_null: function(){
        this.current_travel_spot = null;
      },
      //parent to child -- reset the travels list
      set_new_travels: function(){
        this.items_travels = ['Spain', 'France', 'Germany', 'England', 'Canada', 'Mexico'];
      },
      show_current_values: function(){
        console.log(
          `\nBest Friend: ${this.current_best_friend}\n
         Travel Spot: ${this.current_travel_spot}`);
      }
    }
  }
</script>

<style>
  .demo_container {
    display: flex;
    flex-direction: column;
    padding: 2rem;
    border: 1px solid black;
    width: 100%;
    height: 100%;
    max-width: 60rem;
    min-height: 38rem;
  }
  .buttons_box {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-top: 3rem;
  }
  .selects_box {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-top: 3rem;
  }
  .pbox {
    margin-top: 2.5rem;
  }
  button {
    width: max-content;
  }
</style>