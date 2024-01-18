<template>
  
    
  <img alt="Vue logo" src="./assets/logo.png" />
  <h1>JSON Forms Vue 3 TypeScript Vite</h1>
  <div class="myform">
   

        <json-forms
        :data="data"
        :renderers="renderers"
        :schema="schema"
        :uischema="uischema"
        @change="onChange"
      />

     

      <div v-if="data.isshow">
        
        <kbutton
                    @click="handleDomEvent"
                    @mousedown="handleDomEvent"
                    @mouseup="handleDomEvent"
                    @focus="handleDomEvent"
                    @blur="handleDomEvent"
                    @keypress="handleDomEvent"
                    >
                    NEXT
        </kbutton>
        
    </div>
      
   
  </div>
  
    
 
 
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs } from "vue";
import { JsonForms, JsonFormsChangeEvent } from "@jsonforms/vue";
import {
  defaultStyles,
  mergeStyles,
  vanillaRenderers,
} from "@jsonforms/vue-vanilla";

import jsonschema from './appdata/schema.json'
import jsonuischema from './appdata/uischema.json'

//for kendo
import { Button } from '@progress/kendo-vue-buttons';
import { Input } from "@progress/kendo-vue-inputs";


// mergeStyles combines all classes from both styles definitions into one
const myStyles = mergeStyles(defaultStyles, { control: { label: "mylabel" } });

const renderers = [
  ...vanillaRenderers,
  // here you can add custom renderers
];

const schema = jsonschema;
const uischema = jsonuischema;


export default defineComponent({
  name: "App",
  components: {
    JsonForms,
    'kbutton': Button,
    kinput: Input
  },
  setup(){

    const state = reactive({
      renderers: Object.freeze(renderers), 
      
      data: {
        firstname: "",
        lastname: "",
        isshow: false,
        birthday: "2024-01-01",
        age: 1
      },
      schema,
      uischema

    })

    return {...toRefs(state)}
  },
  methods: {
    onChange(event: JsonFormsChangeEvent) {
      this.data = event.data;
      ///console.log(this.data)

      this.data.age = this.computeAge(this.data.birthday)
      if(this.data.age <= 18)
      {
          this.data.isshow = false
      }
      else{
        this.data.isshow = true
      }
      //console.log(this.data.age)
      
    },
    
    computeAge(dateString:string | number)
    {
        let today = new Date()
        let birthdate = new Date(dateString)
        let age = today.getFullYear() - birthdate.getFullYear()
        let m = today.getMonth() - birthdate.getMonth()
        if (m < 0 || (m === 0 && today.getDate() < birthdate.getDate())) {
          age--;
        }
        return age;

    },

    handleDomEvent (event:any) {
        console.log(event.type);
      }
  },
  provide() {
    return {
      styles: myStyles,
    };
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 120px;
  margin-right: 120px;
}

.mylabel {
  color: darkslategrey;
}

.vertical-layout {
  margin-left: 10px;
  margin-right: 10px;
}

.myform {
  width: 640px;
  margin: 0 auto;
}

.text-area {
  min-height: 80px;
}

</style>
