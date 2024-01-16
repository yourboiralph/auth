<template>
    <div class="flex items-center justify-center h-screen">
      <div class="text-center">
        <h1>Create Question</h1>
        <form @submit.prevent="submitForm">
          <label for="question">Question: {{ state.user && state.user.latest_question_id }}</label>
          <input type="text" id="question" v-model="question" required class="mb-2 p-2 border rounded" />
          <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded">Submit</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        question: "",
        state: {
          user: null,
        },
      };
    },
    mounted() {
      this.fetchUser();
    },
    methods: {
      async submitForm() {
        const url = `http://127.0.0.1:8000/api/questions/${this.state.user.id}`;
  
        try {
          const response = await fetch(url, {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              question: this.question,
            }),
          });
  
          if (response.ok) {
            const data = await response.json();
            console.log(data.message);
            navigateTo('/admin/options/createOptions');
          } else {
            const errorData = await response.json();
            console.error(errorData.error || errorData.message);
          }
        } catch (error) {
          console.error('Error:', error);
        }
      },

      async fetchUser() {
        try {
          const response = await fetch(`http://127.0.0.1:8000/api/admin`, {
            method: 'GET',
            headers: {
              'Authorization': 'Bearer ' + localStorage.getItem('_token')
            }
          });
  
          if (response.ok) {
            const userData = await response.json();
            this.state.user = userData.data;
          } else {
            console.error('Failed to fetch user data');
          }
        } catch (error) {
          console.error('Error:', error);
        }
      },
    },
  };
  </script>
  