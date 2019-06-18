<template>
    <div class="home">
        <img alt="Vue logo" src="../assets/logo.png" />
        <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
        <label class="text-reader">
            <input type="file" v-on:change="loadTextFromFile" />
        </label>

        <ul style="list-style: none; text-align: left; width: 50%; margin: 0px auto;">
            <li v-for="val in uniqueResults">{{ val }}</li>
        </ul>

    </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import * as _ from 'underscore';
const reader = new FileReader();

export default {
    name: "home",
    components: {
        HelloWorld
    },
    data() {
        return {
            uniqueResults: false
        }
    },
    methods: {
        loadTextFromFile(ev) {
            const file = ev.target.files[0];
            const reader = new FileReader();

            reader.onload = e => {
                let extratedData = e.target.result.match(/\$t\(['"][\w\s]+['"]\)/g);

                let results = [];

                extratedData.map(val => {
                    let matchVal = val.match(/(?<=["'])[^"']+(?=["'])/g);
                    results.push(matchVal.join(''));
                })

                this.uniqueResults = _.unique(results);

            
                



            }
            reader.readAsText(file);
        }
    }
};
</script>
