<script setup>
    import { ref, computed } from 'vue';

    let formData = ref({
        name: '',
        email: '',
        password: '',
        passConfirmation: ''
    })

    const isNameValid = computed(() => formData.value.name.trim() !== "")

    const isEmailValid = computed(() =>  /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/.test(formData.value.email) )

    const isPassWordValid = computed(() => formData.value.password.length >= 8)

    const isPassConfirmationValid = computed(() => formData.value.passConfirmation === formData.value.password && formData.value.passConfirmation !== '' && formData.value.passConfirmation.length >= 8)

    const isFormValid = computed(() => isNameValid.value && isEmailValid.value && isPassWordValid.value && isPassConfirmationValid.value)

    const resetForm = () => {
    formData.value = { name: '', email: '', password: '', passConfirmation: '' };
};

    const handleForm = () => {
try {
    console.log(`User ${formData.value.name} submitted the form succsfully`);
    resetForm()
    
} catch (error) {
    console.error(`There was an error: ${error}`);
    
}        
    }


</script>


<template>
    <h1>Form</h1>
    <form @submit.prevent="handleForm">
        
    <label for="name">Name:</label>
    <br>
    <input type="text" name="name" id="name" placeholder="Please type name" v-model="formData.name" required>
    <br>
    <span v-if="!isNameValid" aria-live="polite" class="error">You must enter name</span>
    <span v-else-if="isNameValid" class="correct">✅</span>
    <br>

    <label for="email">Email:</label>
    <br>
    <input type="email" name="email" id="email" placeholder="Please type email" v-model="formData.email" required>
    <br>
    <span v-if="!isEmailValid" aria-live="polite" class="error">Please type valid email</span>
    <span v-else-if="isEmailValid" class="correct">✅</span>

    <br>
    <label for="password">Password:</label>
    <br>
    <input type="password" name="password" id="password" v-model="formData.password" placeholder="Please type password">
    <br>
    <span v-if="!isPassWordValid" aria-live="polite" class="error">Password must be at least 8 char</span>
    <span v-else-if="isPassWordValid" class="correct">✅</span>

    <br>

    <label for="passConfirmation">Confirm Password:</label>
    <br>
    <input type="password" name="passConfirmation" id="passConfirmation" v-model="formData.passConfirmation" placeholder="Please confirm the password">
    <br>
    <span v-if="!isPassConfirmationValid" aria-live="polite" class="error">Must match with password and be at least 8 char</span>
    <span v-else-if="isPassConfirmationValid" class="correct">✅</span>

    <br>

    <button :disabled="!isFormValid" type="submit">Submit</button>
    </form>
</template>

<style scoped>
    .error {
        color: red;
        font-size: 0.9em;
    }

    .correct {
        font-size: 0.9em;
    }
</style>