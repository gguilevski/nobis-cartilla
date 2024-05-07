<script setup>
import { ref, watch } from 'vue'
import { MeiliSearch } from 'meilisearch'
import '../assets/scss/global.scss'

const props = defineProps({
  msg: String,
  myProp: {
    type: Object,
    required: false,
  },
  post: {
    type: Object,
    default: () => ({})
  }

});



const client = new MeiliSearch({
  host: 'http://127.0.0.1:7700',
  apiKey: 'kFoynfybYvocpg8ZDcI52WQTIynX7KkiUD4oOu0bcdc',
})


const question = ref('')
const answers = ref([])
const loading = ref(false)

watch(question, async (newQuestion, oldQuestion) => {

  if (newQuestion) {

    const result = await client.index('nobis-index').search(newQuestion, { hitsPerPage: 10 })

    answers.value = result

  } else { answers.value = [] }


  // if (newQuestion.includes('?')) {
  //   loading.value = true
  //   answer.value = 'Thinking...'
  //   try {

  //     answer.value = 'pepe'
  //   } catch (error) {
  //     answer.value = 'Error! Could not reach the API. ' + error
  //   } finally {
  //     loading.value = false
  //   }
  // }
})

</script>

<template>



  <main>
 


    <section class="section with-background bg-variant">
      <header class="section-header stack gap-2 lg:gap-4">
        <h3>Cartilla Médica</h3>
      </header>


      <div class="gallery">
        <form action="" role="search" class="ais-SearchBox-form">
        <input class="ais-SearchBox-input" type="search" placeholder="" autocomplete="off" autocorrect="off" v-model="question" :disabled="loading" />
      </form>
        <ul class="grid">

          <li class="card" v-for="item in answers.hits" :key="item.id">
            <h2>{{ item.nombre }}</h2>
            <h3>{{ item.rubro }}</h3>
            <h3>{{ item.especialidad }}</h3>
            <p>{{ item.telefono }}</p>
            <p>{{ item.domicilio }}</p>
          </li>

        </ul>



      </div>
    </section>
  </main>



</template>

<style lang="scss" scoped>
// Sass (SCSS) code
$sidebar-width: 250px;

.container {
  display: grid;
  grid-template-columns: $sidebar-width 1fr;
  grid-template-rows: auto;
  grid-template-areas: "sidebar content";
  gap: 20px;
}

.sidebar {
  grid-area: sidebar;
}

.content {
  grid-area: content;
}

.hero {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
}

.roles {
  display: none;
}

.hero img {
  aspect-ratio: 5 / 4;
  object-fit: cover;
  object-position: top;
  border-radius: 1.5rem;
  box-shadow: var(--shadow-md);
}

@media (min-width: 50em) {
  .hero {
    display: grid;
    grid-template-columns: 6fr 4fr;
    padding-inline: 2.5rem;
    gap: 3.75rem;
  }

  .roles {
    margin-top: 0.5rem;
    display: flex;
    gap: 0.5rem;
  }

  .hero img {
    aspect-ratio: 3 / 4;
    border-radius: 4.5rem;
    object-fit: cover;
  }
}

/* ====================================================== */

.section {
  display: grid;
  gap: 2rem;
}

.with-background {
  position: relative;
}

.with-background::before {
  --hero-bg: var(--bg-image-subtle-2);

  content: '';
  position: absolute;
  pointer-events: none;
  left: 50%;
  width: 100%;
  aspect-ratio: calc(2.25 / var(--bg-scale));
  top: 0;
  transform: translateY(-75%) translateX(-50%);
  background-blend-mode: overlay, normal, normal, normal;
  mix-blend-mode: var(--bg-blend-mode);
  z-index: -1;
}

.with-background.bg-variant::before {
  --hero-bg: var(--bg-image-subtle-1);
}

.section-header {
  justify-self: center;
  text-align: center;
  max-width: 50ch;
  font-size: var(--text-md);
  color: var(--gray-300);
}

.section-header h3 {
  font-size: var(--text-2xl);
}

@media (min-width: 50em) {
  .section {
    grid-template-columns: repeat(4, 1fr);
    grid-template-areas: 'header header header header' 'gallery gallery gallery gallery';
    gap: 5rem;
  }

  .section.with-cta {
    grid-template-areas: 'header header header cta' 'gallery gallery gallery gallery';
  }

  .section-header {
    grid-area: header;
    font-size: var(--text-lg);
  }

  .section-header h3 {
    font-size: var(--text-4xl);
  }

  .with-cta .section-header {
    justify-self: flex-start;
    text-align: left;
  }

  .gallery {
    grid-area: gallery;
  }

  .cta {
    grid-area: cta;
  }
}

/* ====================================================== */

.mention-card {
  display: flex;
  height: 7rem;
  justify-content: center;
  align-items: center;
  text-align: center;
  border: 1px solid var(--gray-800);
  border-radius: 1.5rem;
  color: var(--gray-300);
  background: var(--gradient-subtle);
  box-shadow: var(--shadow-sm);
}

@media (min-width: 50em) {
  .mention-card {
    border-radius: 1.5rem;
    height: 9.5rem;
  }
}
</style>
