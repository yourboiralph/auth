<template>
  <div class="pt-10">
    <p>Questions List <button class="action-btn view" @click="navigateTo('/admin/questions/createQuestions')">Add</button></p>
    <table>
      <thead>
        <tr>
          <th>Question</th>
          <th>Admin</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="t in data.questions" :key="t.id">
          <td>
            <nuxt-link :to="'/questions/' + t.id">{{ `${t.question}` }}</nuxt-link>
          </td>
          <td>
            <nuxt-link :to="'/admins/' + t.id">{{ `${t.adminID}` }}</nuxt-link>
          </td>
          <td>
            <button class="action-btn edit" @click="editQuestions(t.id, t.adminID)">Edit</button>
            <button class="action-btn delete" @click="deleteQuestions(t.id, t.adminID)">Delete</button>
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
      const { data } = await useFetch('http://127.0.0.1:8000/api/questions/');

      definePageMeta({
        layout: 'admin-layout',
      });

      return { data };
    } catch (error) {
      console.error('Error fetching:', error);
      // You might want to handle errors more gracefully, for example by showing an error message to the user
    }
  },
  methods: {
    async editQuestions(questionId, adminId) {
  try {
    // Fetch the specific question data
    const { data: questionData } = await useFetch(`http://127.0.0.1:8000/api/questions/${questionId}`);
    
    // Navigate to the edit page with the question data
    this.$router.push({
      path: '/admin/questions/editQuestions',
      query: { questionId, adminId, questionData }
    });
  } catch (error) {
    console.error('Error editing:', error);
    // You might want to handle errors more gracefully, for example by showing an error message to the user
  }
},
    async deleteQuestions(questionId, adminId) {
      try {
        // Make a DELETE request to the API endpoint
        await fetch(`http://127.0.0.1:8000/api/questions/${questionId}/${adminId}/delete`, {
          method: 'DELETE',
        });

        // Refresh the current route
        this.$router.go();
      } catch (error) {
        console.error('Error deleting:', error);
        // You might want to handle errors more gracefully, for example by showing an error message to the user
      }
    },
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
