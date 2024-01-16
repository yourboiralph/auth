<!-- pages/admin/questions/editQuestions.vue -->

<template>
    <div class="flex items-center justify-center min-h-screen">
      <div class="text-center">
        <h1 class="text-2xl font-bold mb-4">Edit Question</h1>
        <form @submit.prevent="submitForm">
          <label class="block mb-2">Question:</label>
          <input v-model="editedQuestion" type="text" class="w-full px-4 py-2 border rounded" required />
          <button type="submit" class="mt-4 px-4 py-2 bg-blue-500 text-white rounded">Save Changes</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    mounted() {
      this.fetchUser();
    },
    data() {
      return {
        editedQuestion: '',
        state: {
          user: null,
        },
      };
    },
    async asyncData({ params }) {
      // Fetch the specific question data based on the route parameters
      const response = await fetch(`http://127.0.0.1:8000/api/questions/${params.questionId}`);
      const data = await response.json();
      return { originalQuestion: data.question };
    },
    methods: {
        async submitForm() {
            try {
    const questionId = this.$route.query.questionId; // Access query parameters

    if (!questionId) {
      console.error('Error updating question: questionId is missing in the route.');
      // Handle the error, for example, show an error message to the user
      return;
    }

    // Make a PUT request to update the question in the database
    await fetch(`http://127.0.0.1:8000/api/questions/${questionId}/${this.state.user.id}/edit`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        question: this.editedQuestion,
      }),
    });

    // Redirect back to the question list page after successful update
    this.$router.push('/admin/questions');
  } catch (error) {
    console.error('Error updating question:', error);
    // Handle errors, for example, show an error message to the user
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
  
  