<template>
    <div class="todo">
        <div>
            <button type="button" @click="Add">Add</button>
        </div>
        <table>
            <tr>
                <th></th>
                <th></th>
                <th>ID</th>
                <th>NAME</th>
            </tr>
            <tr v-for="(d, index) in data" :key="index">
                <td>
                    <button type="button" @click="Edit(index)">Edit</button>
                </td>
                <td>
                    <button type="button" @click="Delete(index)">Delete</button>
                </td>
                <td>{{ d.id }}</td>
                <td>{{ d.name }}</td>
            </tr>
        </table>
        <div>
          <div v-for="value, key in form" :key="key">
            <input type="text" v-model="form[key]" />
          </div>
          <button type="button" @click="Save">Save</button>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Todo',
  data: function () {
    return {
      form: {},
      data: []
    }
  },
  created: function () {
    this.getData()
  },
  methods: {
    Add () {
      this.data.push(
        { id: '', name: '' }
      )
    },
    Edit (index) {
      this.targetindex = index
      this.form = Object.assign({}, this.data[index])
    },
    Save () {
      const path = 'http://localhost:5000/api/' + this.targetindex
      this.data.splice(
        this.targetindex,
        1,
        Object.assign({}, this.form)
      )
      axios.put(path, this.form, { withCredentials: true })
    },
    Delete (index) {
      this.data.splice(index, 1)
    },
    getData () {
      const path = 'http://localhost:5000/api/0'
      axios.get(path, {
        withCredentials: true
      }).then((res) => {
        this.data.push(res.data)
      })
    }
  }
}
</script>
