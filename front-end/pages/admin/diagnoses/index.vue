<template>
  <div>
    <p>Diagnoses List</p>

    <table>
      <thead>
        <tr>
          <th>Diagnosis</th>
          <th>Taker</th>
          <th>Score</th>
          <th>Depression</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <!-- <p>{{ data }}</p> -->
        <tr v-for="t in data.Diagnosis" :key="t.id">
          <td>
            <nuxt-link :to="'/diagnoses/' + t.id">{{ `${t.id}` }}</nuxt-link>
          </td>
          <td>{{ t.taker }}</td>
          <td>{{ t.total_score }}</td>
          <td>{{ t.depression_type.type }}</td>
          <td>
            <button class="action-btn delete" @click="deleteTaker(t.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  async setup() {
    try {
      const { data } = await useFetch('http://127.0.0.1:8000/api/diagnoses');

      definePageMeta({
        layout: 'admin-layout',
      });

      return { data };
    } catch (error) {
      console.error('Error fetching:', error);
      // You might want to handle errors more gracefully, for example by showing an error message to the user
    }
  },

};
</script>

<style>
/* Add your table styles here if needed */
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

.action-btn {
  margin-right: 5px;
  padding: 3px 8px;
  cursor: pointer;
  border: none;
  border-radius: 4px;
}

.view {
  background-color: green;
  color: white;
}

.edit {
  background-color: yellow;
  color: #333;
}

.delete {
  background-color: red;
  color: white;
}
</style>