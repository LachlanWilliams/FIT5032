<template>
    <div class="container mt-5">
        <div class="row">
            <h1 class="text-center">User Information Form</h1>
            <div class="col-md-8 offset-md-2">
            
                <form @submit.prevent="submitForm">
                    <div class="row-mb-3">
                        <div class="col-md-6">
                            <label for="username" class="form-label">Username:</label>
                            <input type="text" class="form-control" id="username" @blur="() => validateName(true)" @input="() => validateName(false)" v-model="formData.username">
                            <div v-if="errors.username" class="text-danger">{{ errors.username }}</div>
                        </div>
                        <div class="col-md-6">
                            <label for="password" class="form-label">Password:</label>
                            <input type="password" class="form-control" id="password" @blur="() => validatePassword(true)" @input="() => validatePassword(false)" v-model="formData.password">
                            <div v-if="errors.password" class="text-danger">{{ errors.password }}</div>
                        </div>
                    </div>
                    <div class="row-mb-3"> 
                        <div class="col-md-6">
                            <div class="form-check">
                                <label for="isAustralian" class="form-label">Australian Resident?</label>
                                <input type="checkbox" v-model="formData.isAustralian">
                            </div>
                        </div>
                        <div class="col-md-6">
                            <label for="gender" class="form-label">Gender</label>
                            <select id="gender" class="form-control" @blur="() => validateGender(true)" @input="() => validateGender(false)" v-model="formData.gender">
                                <option value="female">Female</option>
                                <option value="male">Male</option>
                                <option value="other">Other</option>
                            </select>
                            <div v-if="errors.gender" class="text-danger">{{ errors.gender }}</div>


                        </div>
                    </div>
                    <div class="row-mb-3"> 
                        <label for="reason" class="form-label">Reason For Joining:</label>
                        <textarea id="reason" class="form-control" name="reason" rows="3" @blur="() => validateReason(true)" @input="() => validateReason(false)" v-model="formData.reason"></textarea>
                        <div v-if="errors.reason" class="text-danger">{{ errors.reason }}</div>
                    </div>
                    <div class="text-center">
                        <button type="submit" class="btn btn-primary me-2">Submit</button>
                        <button type="clear" class="btn btn-secondary">Clear</button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <div class="row mt-5" v-if="submittedCards.length">
        <DataTable :value="submittedCards" tableStyle="min-width: 50rem">
            <Column field="username" header="Username"></Column>
            <Column field="password" header="Password"></Column>
            <Column field="isAustralian" header="Australian Resident">
                <template #body="slotProps">
                    {{ slotProps.data.isAustralian ? 'Yes' : 'No' }}
                </template>
            </Column>
            <Column field="gender" header="Gender"></Column>
            <Column field="reason" header="Reason"></Column>
        </DataTable>
    </div>
</template>

<script setup>
// Our logic will go here

    import { ref } from 'vue';
    import DataTable from 'primevue/datatable';
    import Column from 'primevue/column';
    
    const formData = ref({
        username: '',
        password: '',
        isAustralian: false,
        reason: '',
        gender: ''
    });

    const submittedCards = ref([]);

    const submitForm = () => {
        validateName(true);
        validatePassword(true);
        validateGender(true);
        validateReason(true);
        if(!errors.value.username && !errors.value.password && !errors.value.gender && !errors.value.reason) {
            submittedCards.value.push({
                ...formData.value
            });
            //clearForm();
        }
    };

    const errors = ref({
        username: null,
        password: null,
        resident: null,
        gender: null,
        reason: null,
    });

    const validateName = (blur) => {
        if(formData.value.username.length < 3) {
            if (blur) errors.value.username = "Name must be at least 3 characters";
        } else {
            errors.value.username = null;
        }
    };

    const validatePassword = (blur) => {
        const password = formData.value.password;
        const minlength = 8;
        const hasUppercase = /[A-Z]/.test(password);
        const hasLowercase = /[a-z]/.test(password);
        const hasNumber = /\d/.test(password);
        const hasSpecialChar = /[!@#$%^&*(),.?":{}|<>"]/.test(password);

        if(password < minlength){
            if (blur) errors.value.password = `Password must be at least ${minlength} characters long.`;
        } else if (!hasUppercase) {
            if (blur) errors.value.password = `Password must contain at least one uppercase letter.`;
        }else if (!hasLowercase) {
            if (blur) errors.value.password = `Password must contain at least one lowercase letter.`;
        }else if (!hasNumber) {
            if (blur) errors.value.password = `Password must contain at least one Number.`;
        }else if (!hasSpecialChar) {
            if (blur) errors.value.password = `Password must contain at least one special character.`;
        } else {
            errors.value.password = null;
        }

    };

    const validateGender = (blur) => {
        if(formData.value.gender == '' ) {
            if (blur) errors.value.gender = "Select from the following list.";
        } else {
            errors.value.gender = null;
        }
    };

    const validateReason = (blur) => {
        const minlength = 10;
        const maxlength = 50;

        if(formData.value.reason.length < minlength ) {
            if (blur) errors.value.reason = `Your reason needs to be at least ${minlength} characters long.`;
        } else if (formData.value.reason.length > maxlength ) {
            if (blur) errors.value.reason = `Your reason needs to be at less than ${maxlength} characters long.`;
        } else {
            errors.value.reason = null;
        }
    };



</script>

<style scoped>
    .card {
    border: 1px solid #ccc;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    .card-header {
    background-color: #275FDA;
    color: white;
    padding: 10px;
    border-radius: 10px 10px 0 0;
    }
    .list-group-item {
    padding: 10px;
    }

    /* Add media queries for breakpoints */
    @media (max-width: 768px) {
    .card {
        width: 100%;

    }
    .card-header {
    background-color: #2ad67d;
    }
    }

    @media (max-width: 576px) {
    .container {
        padding: 0 10px;
    }
    .card-header {
    background-color: #d6802a;
    }
    }
</style>