<template>
  <div>
    <p>Admins List</p>

    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Age</th>
          <th>Email</th>
          <th>Verification Status</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="t in data" :key="t.id">
          <td>
            <nuxt-link :to="'/admins/' + t.id">{{ `${t.first_name} ${t.last_name}` }}</nuxt-link>
          </td>
          <td>{{ t.age }}</td>
          <td>{{ t.email }}</td>
          <td>{{ t.is_verified ? 'Verified' : 'Not Verified' }}</td>
          <td>
            <button class="action-btn delete" @click="deleteTaker(t.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>

</style>

<script>
import { ref, onMounted } from 'vue';

export default {
  async setup() {
    const data = ref(null);

    // Check if user is logged in on component mount
    onMounted(() => {
      checkLogged();
      fetchUser();
    });

    async function checkLogged() {
      if (!localStorage.getItem('_token')) {
        navigateTo('/login');
      }
    }

    definePageMeta({
          layout: 'admin-layout',
        }); 

    async function fetchUser() {
      try {
        const response = await fetch(`http://127.0.0.1:8000/api/takers/`, {
          method: 'GET',
        });

        if (response.ok) {
          const userData = await response.json();
          console.log(userData);
          data.value = userData.takers;
        } else {
          console.error('Failed to fetch user data');
        }
      } catch (error) {
        console.error('Error:', error);
      }
    }

    return {
      data,
    };
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