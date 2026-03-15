<template>
  <nav v-if="user" class="navbar">
    <div class="navbar-brand">
      <router-link to="/dashboard" class="brand-link">
        🚀 SubConnect
      </router-link>
    </div>

    <div class="navbar-menu">
      <span class="user-email">{{ user.email }}</span>
      <button @click="handleLogout" class="logout-btn">Log Out</button>
    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import { getAuth, onAuthStateChanged, signOut } from 'firebase/auth';

const router = useRouter();
const auth = getAuth();
const user = ref(null);

// This listens to Firebase. If the user state changes, it updates the navbar instantly.
onMounted(() => {
  onAuthStateChanged(auth, (currentUser) => {
    user.value = currentUser;
  });
});

const handleLogout = async () => {
  try {
    console.log("Logging out...");
    await signOut(auth);
    router.push('/login'); 
  } catch (error) {
    console.error("Logout Error:", error);
  }
};
</script>

<style scoped>
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  background-color: #ffffff;
  border-bottom: 1px solid #eaeaea;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
}

.navbar-brand {
  font-size: 1.5rem;
  font-weight: bold;
}

.brand-link {
  text-decoration: none;
  color: #333;
}

.brand-link:hover {
  color: #007bff;
}

.navbar-menu {
  display: flex;
  align-items: center;
  gap: 1.5rem; /* Puts space between the email and the button */
}

.user-email {
  color: #666;
  font-size: 0.95rem;
}

.logout-btn {
  padding: 0.5rem 1rem;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.2s;
}

.logout-btn:hover {
  background-color: #c82333;
}
</style>