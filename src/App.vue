<template>
  <div id="app">
    <h1>Instant Search</h1>
    <div class="magic" v-html="magic"></div>
    <form id="main">
      <div class="bar">
        <input type="text" placeholder="Enter some filter criteria..." v-model="filter" 
          @keyup.down="test('down', filteredTopics)"
          @keyup.up="test('up', filteredTopics)"
          @keypress="newtest($event)">
        <ul>
          <li v-for="(topic, index) in filteredTopics" :key="index"
            :class="[
              index == currentIndex ? 'test' : ''
              ]"
          >
          <span v-html="topic" />
          </li>
        </ul>
      </div>

    </form>
  </div>
</template>

<script>

export default {
  name: 'app',
  components: {
  },
  data(){
    return{
      topics: [
        "Vue is a progressive framework for building user interfaces",
        "Every Vue application starts by creating a new Vue instance with the Vue function",
        "Learn HTML and CSS along with Javascript",
        "Learn Vue and Vuex",
        "Build something awesome",
        "The object syntax for v-bind:style is pretty straightforward - it looks almost like CSS, except it’s a JavaScript object",
        "Vue.js uses an HTML-based template syntax that allows you to declaratively bind the rendered DOM to the underlying Vue instance’s data"
      ],
      filter: "",
      currentTopic: "",
      currentIndex: 0,
      magic: "test <b>mag</b>ic"
    }
  },
  methods:{
    newtest(e){
      console.log(">>>>", e.keyCode);
    },
    test(key, filteredTopics){
console.log(">>> test", filteredTopics, this.currentIndex);
if(key == "down")
  {
    this.currentIndex++;
  }
else
  {
    this.currentIndex--;
  }
//this.currentIndex = (key == "down") ? this.currentIndex++ : this.currentIndex--;
    }
  },
  computed: {
    filteredTopics(){
      if(!this.filter){
        return this.topics;
      }
      var temp_array = [];
      var j=0;
      for(var i=0; i<this.topics.length; i++){
        if(this.topics[i].toLowerCase().indexOf(this.filter.toLowerCase(), 0) >= 0){          
          temp_array[j] = this.topics[i];

          //var regex = new RegExp(this.filter, "gi");
          temp_array[j] = temp_array[j].replace(new RegExp(this.filter, "gi"), match => {
                    return '<span class="highlightText">' + match + '</span>';
                });

          j++;
        }
      }    
      //this.currentTopic = temp_array[0];
      return temp_array;
    }
  }
}
</script>

<style>
.test{
  background-color: red !important;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.bar{
    background-color:#5c9bb7;
    box-shadow: 0 1px 1px #ccc;
    border-radius: 2px;
    width: 400px;
    padding: 14px;
    margin: 45px auto 20px;
    position:relative;
}
.bar input{
    background:#fff no-repeat 13px 13px;
    background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyBpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMC1jMDYwIDYxLjEzNDc3NywgMjAxMC8wMi8xMi0xNzozMjowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNSBXaW5kb3dzIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkU5NEY0RTlFMTA4NzExRTM5RTEzQkFBQzMyRjkyQzVBIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkU5NEY0RTlGMTA4NzExRTM5RTEzQkFBQzMyRjkyQzVBIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6RTk0RjRFOUMxMDg3MTFFMzlFMTNCQUFDMzJGOTJDNUEiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6RTk0RjRFOUQxMDg3MTFFMzlFMTNCQUFDMzJGOTJDNUEiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz4DjA/RAAABK0lEQVR42pTSQUdEURjG8dOY0TqmPkGmRcqYD9CmzZAWJRHVRIa0iFYtM6uofYaiEW2SRJtEi9YxIklp07ZkWswu0v/wnByve7vm5ee8M+85zz1jbt9Os+WiGkYdYxjCOx5wgFeXUHmtBSzpcCGa+5BJTCjEP+0nKWAT8xqe4ArPGEEVC1hHEbs2oBwdXkM7mj/JLZrad437sCGHOfUtcziutuYu2v8XUFF/4f6vMK/YgAH1HxkBYV60AR31gxkBYd6xAeF3VzMCwvzOBpypX8V4yuFRzX2d2gD/l5yjH4fYQEnzkj4fae5rJulF2sMXVrAsaTWttRFu4Osb+1jEDT71/ZveyhouTch2fINQL9hKefKjuYFfuznXWzXMTabyrvfyIV3M4vhXgAEAUMs7K0J9UJAAAAAASUVORK5CYII=);

    border: none;
    width: 100%;
    line-height: 19px;
    padding: 11px 0;

    border-radius: 2px;
    box-shadow: 0 2px 8px #c4c4c4 inset;
    text-align: left;
    font-size: 14px;
    font-family: inherit;
    color: #738289;
    font-weight: bold;
    outline: none;
    text-indent: 40px;
}
ul{
    list-style: none;
    /*width: 428px;*/
    margin: 0 auto;
    text-align: left;
}

ul li{
    border-bottom: 1px solid #ddd;
    padding: 10px;
    overflow: hidden;
}
.highlightText{
  background: yellow;
}

.magic b {
  color: #262626;
}
</style>
