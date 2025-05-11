<script setup>
import { ref } from 'vue'

// Citations triées par catégories
const citationsParCategorie = {
  motivation: [
    "Ils ne savaient pas que c’était impossible, alors ils l’ont fait. – Mark Twain",
    "Le seul endroit où le succès vient avant le travail, c’est dans le dictionnaire. – Vidal Sassoon",
    "Tout ce que l’esprit de l’homme peut concevoir et croire, il peut l’accomplir. – Napoleon Hill"
  ],
  amour: [
    "Aimer, ce n’est pas se regarder l’un l’autre, c’est regarder ensemble dans la même direction. – Antoine de Saint-Exupéry",
    "On ne voit bien qu’avec le cœur. L’essentiel est invisible pour les yeux. – Saint-Exupéry",
    "L’amour est la seule chose qu’on emporte dans l’éternité. – Jean d’Ormesson"
  ],
  humour: [
    "Je suis multitâche : je peux écouter, ignorer et oublier en même temps. – Anonyme",
    "Le travail d’équipe est essentiel. Ça permet de toujours rejeter la faute sur quelqu’un d’autre. – Anonyme",
    "La motivation, c’est comme le café : ça ne dure pas longtemps, il faut en reprendre souvent. – Anonyme"
  ]
}

// Réactivité des données
const categorieActive = ref("motivation")
const citation = ref("Choisissez une catégorie puis cliquez sur 'Nouvelle citation'")
const copieConfirmee = ref(false)  // état de confirmation de copie
const showModal = ref(false)       // état de la modal de partage

// Génère une citation aléatoire selon la catégorie sélectionnée
function genererCitation() {
  const liste = citationsParCategorie[categorieActive.value]
  const index = Math.floor(Math.random() * liste.length)
  citation.value = liste[index]
}

// Copie la citation dans le presse-papiers et affiche un message de confirmation
function copierCitation() {
  navigator.clipboard.writeText(citation.value)
    .then(() => {
      copieConfirmee.value = true
      setTimeout(() => {
        copieConfirmee.value = false
      }, 2000)
    })
    .catch(() => {
      console.error("Erreur lors de la copie.")
    })
}

// Affiche la modal de partage
function partagerCitation() {
  showModal.value = true
}
function closeModal() {
  showModal.value = false
}

// Partage sur les réseaux sociaux
function shareToFacebook() {
  const text = encodeURIComponent(citation.value)
  const url = `https://www.facebook.com/sharer/sharer.php?u=https://example.com&quote=${text}`
  window.open(url, '_blank')
}
function shareToWhatsApp() {
  const text = encodeURIComponent(citation.value)
  const url = `https://wa.me/?text=${text}`
  window.open(url, '_blank')
}
</script>

<template>
  <div class="container">
    <!-- Image d'en-tête -->
    <img src="/img/image1.jpeg" alt="image1" class="banniere" />
    <h1>Générateur de Citations</h1>

    <!-- Filtres de catégorie -->
    <div class="filtres">
      <button @click="categorieActive = 'motivation'">💪 Motivation</button>
      <button @click="categorieActive = 'amour'">❤️ Amour</button>
      <button @click="categorieActive = 'humour'">😂 Humour</button>
    </div>

    <!-- Affichage de la citation -->
    <transition name="fade" mode="out-in">
      <div class="citation-card" :key="citation">
        <p>{{ citation }}</p>
      </div>
    </transition>

    <!-- Boutons d'action -->
    <div class="actions">
      <button @click="genererCitation">Nouvelle citation</button>

      <div class="copier-wrapper">
        <button @click="copierCitation" class="copier">Copier</button>
        <!-- Message de confirmation -->
        <span v-if="copieConfirmee" class="copie-message">Citation copiée</span>
      </div>

      <button @click="partagerCitation" class="partager">Partager</button>
    </div>

    <!-- Modal de partage -->
    <div v-if="showModal" class="modal-backdrop" @click.self="closeModal">
      <div class="modal">
        <h3>Partager la citation</h3>
        <div class="share-icons">
          <img src="/img/facebook.png" alt="Facebook" @click="shareToFacebook" />
          <img src="/img/whatsapp.png" alt="WhatsApp" @click="shareToWhatsApp" />
        </div>
        <button @click="closeModal" class="close">Fermer</button>
      </div>
    </div>
  </div>
</template>


<style scoped>
/* Conteneur principal */
.container {
  max-width: 600px;
  margin: 70px auto;
  text-align: center;
  font-family: sans-serif;
  background-color: #121212;
  color: #eeeeee;
  padding: 3rem;
  border-radius: 12px;
}

/* 🖼️ Image d'en-tête */
.banniere {
  width: 100%;
  max-height: 300px;
  object-fit: cover;
  border-radius: 12px;
  margin-bottom: 2%;
}

/* Bloc de citation */
.citation-card {
  background: #1e1e1e;
  padding: 4rem;
  border-radius: 12px;
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.1);
  margin-bottom: 1.5rem;
}

/* Boutons d'action */
.actions {
  display: flex;
  justify-content: center;
  gap: 1rem;
  align-items: center;
}

/* Style des boutons */
button {
  padding: 0.7rem 2rem;
  background: #b4e197; /* vert pastel */
  color: #000;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.3s;
}
button:hover {
  background: #dcd6f7; /* violet pastel au survol */
}
button:active {
  transform: scale(0.96);
}

/* Message temporaire "copié" */
.copier-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.copie-message {
  font-size: 0.8rem;
  color: #a0f1a0;
  margin-top: 0.2rem;
  animation: fadeIn 0.3s ease-in-out;
}
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

/* Animation citation */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.15s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}

/* Modal de partage */
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}
.modal {
  background: white;
  color: #121212;
  padding: 2rem;
  border-radius: 12px;
  text-align: center;
  width: 90%;
  max-width: 400px;
}
.share-icons {
  display: flex;
  justify-content: space-around;
  margin: 1rem 0;
}
.share-icons img {
  width: 40px;
  height: 40px;
  cursor: pointer;
  transition: transform 0.2s;
}
.share-icons img:hover {
  transform: scale(1.2);
}
button.close {
  margin-top: 1rem;
  background-color: #ddd;
  color: #333;
  border: none;
  padding: 0.4rem 1rem;
  border-radius: 6px;
}

/* Responsive mobile */
@media (max-width: 600px) {
  .container {
    padding: 1rem;
    margin: 20px auto;
  }
  .citation-card {
    padding: 1.2rem;
  }
  h1 {
    font-size: 1.5rem;
  }
  .actions {
    flex-direction: column;
    gap: 0.8rem;
  }
  button {
    width: 100%;
  }
  .banniere {
    max-height: 200px;
  }
}

/* Filtres catégorie */
.filtres {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 1rem;
  flex-wrap: wrap;
}
.filtres button {
  padding: 0.4rem 1rem;
  background-color: #333;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}
.filtres button:hover {
  background-color: #555;
}
</style>