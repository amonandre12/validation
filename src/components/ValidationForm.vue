<script setup>
import Form from './component/Form.vue'
import { reactive, ref } from 'vue'

// Données du formulaire
const form = reactive({
  name: "", 
  age: "", 
  email: "",
  selected: "", 
  checkedNames: [],
})

const message = ref("")
const radioName = ref("")
const alert1 = ref("")
const alert2 = ref("")

const options = ref([
  { text: 'Student', value: 'Student' },
  { text: 'Intern', value: 'Intern' },
  { text: 'Professional', value: 'Professional' },
  { text: 'Autre', value: 'Autre' },
])

const onSubmit = () => {
  if (!form.name || !form.age || !form.email || !form.selected || form.checkedNames.length === 0 || !radioName.value || !message.value) {
    alert2.value = "Veuillez remplir tous les champs requis !"
    alert1.value = ""
    // return ;  

    // Alert rouge disparait

     setTimeout(() => {
      alert2.value = ""
    }, 1000)

    return
 } 

  // Affichage des données
  console.log("Nom :", form.name)
  console.log("Age:", form.age)
  console.log("Email :", form.email)
  console.log("Rôle :", form.selected)
  console.log("Recommandation :", radioName.value)
  console.log("Technos connues :", form.checkedNames)
  console.log("Commentaire :", message.value)

  alert1.value = "Formulaire envoyé avec succès !"
  alert2.value = ""

  
  // Alert verte disparait 
    setTimeout(() => {
    alert1.value = ""
  }, 1000)

  

  // Réinitialisation des champs

  form.name = ""
  form.age = ""
  form.email = ""
  form.selected = ""
  form.checkedNames = []
  radioName.value = ""
  message.value = ""

// Call to function 
  submitForm() ;

}


// Fonction pour envoyer les données au back-end
const envoyerFormulaire = async () => {
  const donnees = {
    nom: form.name,
    age: form.age,
    email: form.email,
    role: form.selected,
    technos: form.checkedNames,
    recommandation: radioName.value,
    commentaire: message.value,
  }

  try {
    const response = await fetch('http://localhost:3000/api/contact', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(donnees),
    })

    const result = await response.json()

    if (response.ok) {
      alert1.value = "Message envoyé avec succès !"
      alert2.value = ""

      // Reset du formulaire si tu veux :
      name.value = ""
      email.value = ""
      message.value = ""
    } else {
      alert2.value = result.message || "Une erreur s’est produite."
    }
  } catch (error) {
    console.error("Erreur réseau :", error)
    
  }
}

const submitForm = () => {
  if (name.value && email.value && message.value) {
    envoyerFormulaire()
  } 
}
</script>


<template>
  <Form>
    <template #title>
      Validation de formulaire 
    </template>
  </Form>

  <form @submit.prevent="onSubmit">
    <Form class="form-control">
      <label for="name">Name</label>
      <input type="text" id="name" v-model.trim="form.name" placeholder="Enter your name" />
    </Form>

    <Form class="form-control">
      <label for="email">Email</label>
      <input type="email" id="email" v-model.trim="form.email" placeholder="Enter your email" />
    </Form>

    <Form class="form-control">
      <label for="age">Age</label>
      <input type="number" id="age" v-model.number="form.age" placeholder="Enter your age" />
    </Form>

    <Form class="form-control">
      <label for="role">Which option best describes you?</label>
      <select v-model="form.selected" id="role">
        <option disabled value="">Sélectionner une option</option>
        <option v-for="option in options" :key="option.value" :value="option.value">
          {{ option.text }}
        </option>
      </select>
      <div>Selected: {{ form.selected }}</div>
    </Form>

    <Form class="form-control">
      <label>Recommend us to a friend?</label><br />
      <label>
        <input type="radio" value="Yes" v-model="radioName" name="recommend" />
        Oui
      </label><br />
      <label>
        <input type="radio" value="No" v-model="radioName" name="recommend" />
        Non
      </label><br />
      <label>
        <input type="radio" value="Maybe" v-model="radioName" name="recommend" />
        Peut-être
      </label>
    </Form>

    <Form class="form-control">
      <label>Languages and Frameworks known</label><br />
      <label><input type="checkbox" value="C" v-model="form.checkedNames" /> C</label><br />
      <label><input type="checkbox" value="C++" v-model="form.checkedNames" /> C++</label><br />
      <label><input type="checkbox" value="C#" v-model="form.checkedNames" /> C#</label><br />
      <label><input type="checkbox" value="Java" v-model="form.checkedNames" /> Java</label><br />
      <label><input type="checkbox" value="Python" v-model="form.checkedNames" /> Python</label><br />
      <label><input type="checkbox" value="JavaScript" v-model="form.checkedNames" /> JavaScript</label><br />
      <label><input type="checkbox" value="React" v-model="form.checkedNames" /> React</label><br />
      <label><input type="checkbox" value="Angular" v-model="form.checkedNames" /> Angular</label><br />
      <label><input type="checkbox" value="Django" v-model="form.checkedNames" /> Django</label><br />
      <label><input type="checkbox" value="Spring" v-model="form.checkedNames" /> Spring</label>
    </Form>

    <Form class="form-control">
      <label for="comment">Any comments or suggestions</label>
      <textarea id="comment" v-model="message" placeholder="Enter your comment here"></textarea>
    </Form>

    <button type="submit">Submit</button>

    <p v-if="alert1">{{ alert1 }}</p>
    <p v-if="alert2" style="color: red">{{ alert2 }}</p>
  </form>
</template>


<style scoped>

p {
  color: #05c46b;
}


form {
  background-color: #fff;
  max-width: 500px;
  margin: 50px auto;
  padding: 30px 20px;
  box-shadow: 2px 5px 10px rgba(0, 0, 0, 0.5);
  border-radius: 10px;
}
.form-control {
  text-align: left;
  margin-bottom: 25px;
}
.form-control label {
  display: block;
  margin-bottom: 10px;
}
.form-control input,
.form-control select,
.form-control textarea {
  border: 1px solid #777;
  border-radius: 2px;
  font-family: inherit;
  padding: 10px;
  display: block;
  width: 95%;
}
.form-control input[type="radio"],
.form-control input[type="checkbox"] {
  display: inline-block;
  width: auto;
}
button {
  background-color: #05c46b;
  border: 1px solid #777;
  border-radius: 2px;
  font-family: inherit;
  font-size: 21px;
  display: block;
  width: 100%;
  margin-top: 50px;
  margin-bottom: 20px;
  cursor: pointer;
  transition: background-color 0.3s;
  color: white;
}


button:hover {
  background-color: rgb(12, 241, 115);
}
</style>
