<template>
    <div class="flex items-center justify-center h-screen">
      <div class="text-center">
        <h1>Create Option</h1>
        <form @submit.prevent="submitForm">
          <label for="option">Option:</label>
          <input type="text" id="option" v-model="option" required class="mb-2 p-2 border rounded" />
          <label for="score">Score:</label>
          <input type="number" id="score" v-model="score" required class="mb-2 p-2 border rounded" />
          <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded">Submit</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        option: "",
        score: 0,
        adminId: null,
        questionId: null,
      };
    },
    async mounted() {
      // Fetch adminId and questionId when the component is mounted
      await this.fetchAdminAndQuestionIds();
    },
    methods: {
      async fetchAdminAndQuestionIds() {
        try {
          // Fetch adminId from your API
          const adminResponse = await fetch("http://127.0.0.1:8000/api/admin", {
            method: "GET",
            headers: {
              Authorization: "Bearer " + localStorage.getItem("_token"),
            },
          });
  
          if (adminResponse.ok) {
            const adminData = await adminResponse.json();
            this.adminId = adminData.data.id;
  
            // Fetch the latest questionId for the admin
            await this.fetchLatestQuestionId();
          } else {
            console.error("Failed to fetch admin data");
          }
        } catch (error) {
          console.error("Error:", error);
        }
      },
  
      async fetchLatestQuestionId() {
  try {
    // Fetch the latest questionId for the admin from your API
    const questionResponse = await fetch(`http://127.0.0.1:8000/api/questions`, {
      method: "GET",
    });

    if (questionResponse.ok) {
      const responseData = await questionResponse.json();

      if (responseData.questions && responseData.questions.length > 0) {
        const latestQuestionId = responseData.questions[responseData.questions.length - 1].id;
        console.log("Latest Question ID:", latestQuestionId);
        this.questionId = latestQuestionId;

        // You can now use the 'latestQuestionId' as needed
      } else {
        console.warn("No questions found in the response");
      }
    } else {
      console.error("Failed to fetch latest question data");
    }
  } catch (error) {
    console.error("Error:", error);
  }
},
  
      async submitForm() {
        if (this.adminId && this.questionId) {
          const url = `http://127.0.0.1:8000/api/options/${this.adminId}`;
  
          try {
            const response = await fetch(url, {
              method: "POST",
              headers: {
                "Content-Type": "application/json",
              },
              body: JSON.stringify({
                questionID: this.questionId,
                option: this.option,
                score: this.score,
              }),
            });
  
            if (response.ok) {
              const data = await response.json();
              console.log(data.message);
            } else {
              const errorData = await response.json();
              console.error(errorData.error || errorData.message);
            }
          } catch (error) {
            console.error("Error:", error);
          }
        } else {
          console.error("AdminId or QuestionId not available");
        }
      },
    },
  };
  </script>
  