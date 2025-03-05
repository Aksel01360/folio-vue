<template>
  <div class="portfolio">
    <h1>{{ info.name }}</h1>
    <p>{{ info.description }}</p>

    <h2>Mes projets</h2>
    <ul>
      <li v-for="project in info.projects" :key="project.title">
        <strong>{{ project.title }}</strong>: {{ project.description }}
        <button @click="openModal(project)">Détails</button>
      </li>
    </ul>

    <div v-if="showModal1" class="modal-overlay" @click.self="closeModal">
      <div class="modal-content">
        <h2>{{ project1.title }}</h2>
        <p><strong>Date de création:</strong> {{ project1.date }}</p>
        <p><strong>Technologies utilisées:</strong> {{ project1.technologies }}</p>
        <p>
          <strong>Visiter :</strong>
          <a :href="project1.link" target="_blank">Accéder au projet</a>
        </p>
        <p v-if="project1.github">
          <strong>Repository GitHub :</strong>
          <a :href="project1.github" target="_blank">Voir le code</a>
        </p>
        <button @click="closeModal">Fermer</button>
      </div>
    </div>

    <div v-if="showModal2" class="modal-overlay" @click.self="closeModal">
      <div class="modal-content">
        <h2>{{ project2.title }}</h2>
        <p><strong>Date de création:</strong> {{ project2.date }}</p>
        <p><strong>Technologies utilisées:</strong> {{ project2.technologies }}</p>
        <p>
          <strong>Visiter :</strong>
          <a :href="project2.link" target="_blank">Accéder au projet</a>
        </p>
        <p v-if="project2.github">
          <strong>Repository GitHub :</strong>
          <a :href="project2.github" target="_blank">Voir le code</a>
        </p>
        <button @click="closeModal">Fermer</button>
      </div>
    </div>

    <h2>Contact</h2>
    <ul>
      <li v-for="contact in info.contact" :key="contact.method">
        <strong>{{ contact.method }}:</strong> 
        <a :href="getContactLink(contact)" target="_blank">{{ contact.detail }}</a>
      </li>
    </ul>

    <h2>Contactez-moi</h2>
    <form @submit.prevent="sendEmail" class="contact-form">
      <div>
        <label for="name">Nom / Prénom</label>
        <input type="text" id="name" v-model="form.name" required />
      </div>
      
      <div>
        <label for="subject">Objet</label>
        <input type="text" id="subject" v-model="form.subject" required />
      </div>
      
      <div>
        <label for="message">Message</label>
        <textarea id="message" v-model="form.message" required></textarea>
      </div>
      
      <button type="submit" class="submit-button">Envoyer</button>
    </form>
    <p v-if="successMessage" class="success-message">{{ successMessage }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import info from '@/data/portfolioinfo.js';

const form = ref({
  name: '',
  subject: '',
  message: ''
});

const successMessage = ref('');

const sendEmail = async () => {
  const emailTo = import.meta.env.VITE_CONTACT_EMAIL;

  if (!emailTo) {
    alert("L'adresse email de réception n'est pas définie.");
    return;
  }

  const mailtoLink = `mailto:${emailTo}?subject=${encodeURIComponent(form.value.subject)}&body=${encodeURIComponent(`Nom: ${form.value.name}%0D%0A%0D%0A${form.value.message}`)}`;
  
  window.location.href = mailtoLink;
  successMessage.value = "Votre message a bien été préparé dans votre client mail.";
};

const getContactLink = (contact) => {
  if (contact.method === 'Email') {
    return `mailto:${contact.detail}`;
  }
  return contact.detail;
};

const showModal1 = ref(false);
const showModal2 = ref(false);

const project1 = {
  title: 'Projet 1',
  date: '2024-01-15',
  technologies: 'Vue.js, HTML, CSS',
  link: 'file:///C:/Users/Aks/Desktop/cv%20iconique/cv-iconique/index.html',
  github: ''
};

const project2 = {
  title: 'Projet 2',
  date: '2024-02-10',
  technologies: 'PDF Document',
  link: 'file:///C:/Users/Aks/Documents/cahier%20des%20charges%20pdf.pdf',
  github: ''
};

const openModal = (project) => {
  if (project.title === 'Projet 1') {
    showModal1.value = true;
  } else if (project.title === 'Projet 2') {
    showModal2.value = true;
  }
};

const closeModal = () => {
  showModal1.value = false;
  showModal2.value = false;
};
</script>

<style scoped>
.portfolio {
  max-width: 800px;
  margin: auto;
  text-align: center;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  max-width: 500px;
}
</style>

  
  
  
  
  
  