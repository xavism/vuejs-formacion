<template>
  <div id="tableDemo">
    <h1>{{msg}}</h1>
    <div class="container">
        <table class="table">
            <thead>
                <tr>
                    <th>id</th>
                    <th>Brand</th>
                    <th>Model</th>
                    <th>Power</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(car, index) in cars" :key="car.id">
                    <td><span class="tag">{{car.id}}</span></td>
                    <td v-if="isEditable(index)">
                        <input v-model="car.brand" type="text">
                    </td>
                    <td v-else>{{car.brand}}</td>
                    <td v-if="isEditable(index)">
                        <input v-model="car.model" type="text">
                    </td>
                    <td v-else>{{car.model}}</td>
                    <td v-if="isEditable(index)">
                        <input v-model="car.power" type="number">
                    </td>
                    <td v-else>
                        <PowerPill :power="car.power"></PowerPill>
                    </td>
                    <td>
                        <a @click="edit(index)" class="button is-link is-rounded">{{isEditable(index) ? 'Finish' : 'Edit'}}</a>
                        <a @click="remove(index)" class="button is-danger is-rounded">Delete</a>
                    </td>
                </tr>
                <tr>
                    <td><span class="tag">{{currentId}}</span></td>
                    <td><input v-model="newCar.brand" type="text"></td>
                    <td><input v-model="newCar.model" type="text"></td>
                    <td><input v-model="newCar.power" type="number"></td>
                    <td><a @click="add()" class="button is-primary is-rounded">Add</a></td>
                </tr>
            </tbody>
        </table>
        <p>Total Fields: {{totalRows}}</p>
    </div>
  </div>
</template>

<script>
import PowerPill from '@/components/PowerPill.vue'

export default {
  name: 'TableDemo',
  components: {
      PowerPill
  },
  data() {
      return {
          editables: [],
          currentId: 1,
          newCar: {
              id: 1,
              brand: '',
              model: '',
              power: null
          },
          cars: []
      }
  },
  props: {
      msg: String
  },
  computed: {
      totalRows() {
          return this.cars.length;
      }
  },
  watch: {
      totalRows: function(){
        //   this.$notify({
        //         group: 'alerts',
        //         title: 'Deleted',
        //         text: 'Total rows have changed',
        //         type: 'success'
        //     });
      }
  },
  methods: {
      isEditable(index) {
          return this.editables.includes(index);
      },
      edit(index) {
          if(this.editables.includes(index)) this.editables.splice(this.editables.indexOf(index), 1);
          else this.editables.push(index);
      },
      generateNewCar(id) {
          return { id: id, brand: '', model: '', power: null };
      },
      remove (index) {
          this.cars.splice(index, 1);
          this.$notify({
                group: 'alerts',
                title: 'Deleted',
                text: 'Car deleted Successfully',
                type: 'success'
            });
      },
      add() {
          this.cars.push(this.newCar);
          ++this.currentId;
          this.newCar = this.generateNewCar(this.currentId);
          this.$notify({
                group: 'alerts',
                title: 'Deleted',
                text: 'Car created Successfully',
                type: 'success'
            });
      }
  },
  beforeCreate() {
    this.$notify({
        group: 'alerts',
        title: 'Lifecycle Hook',
        text: 'Im in before created',
        type: 'info'
    });
  },
  created() {
    this.$notify({
        group: 'alerts',
        title: 'Lifecycle Hook',
        text: 'Ive been created',
        type: 'info'
    });
  },
  beforeMount() {
    this.$notify({
        group: 'alerts',
        title: 'Lifecycle Hook',
        text: 'Im in before mount',
        type: 'success'
    });
  },
  mounted() {
    this.$notify({
        group: 'alerts',
        title: 'Lifecycle Hook',
        text: 'Ive been mounted',
        type: 'success'
    });
  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table {
    margin: 0 auto;
}
</style>
