<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-success fw-bold">Edit Contact</p>
                <p class="fst-italic">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Sint quas earum totam, eos
                    consequatur consectetur quis hic veritatis, in aliquid mollitia maxime quo neque nulla cupiditate
                    deserunt adipisci! Fugit, ut.</p>
            </div>
        </div>
    </div>

    <!-- Spinner -->

    <div v-if="loading">
        <div class="container">
            <div class="row">
                <div class="col">
                    <SpinnerVue />
                </div>
            </div>
        </div>
    </div>

    <!-- Error Message -->

    <div v-if="!loading && errorMessage">
        <div class="container mt-3">
            <div class="row">
                <div class="col">
                    <p class="h3 text-danger fw-bold">{{ errorMessage }}</p>
                </div>
            </div>
        </div>
    </div>

    <div class="container mt-3">
        <div class="row">
            <div class="col-md-4">
                <form @submit.prevent="updateSubmit()">
                    <div class="mb-2">
                        <input v-model="contact.name" type="text" class="form-control" placeholder="Name" required> 
                    </div>
                    <div class="mb-2">
                        <input v-model="contact.photo" type="text" class="form-control" placeholder="Photo URL" required>
                    </div>
                    <div class="mb-2">
                        <input v-model="contact.email" type="email" class="form-control" placeholder="Email" required>
                    </div>
                    <div class="mb-2">
                        <input v-model="contact.mobile" type="Number" class="form-control" placeholder="Mobile" required>
                    </div>
                    <div class="mb-2">
                        <input v-model="contact.company" type="text" class="form-control" placeholder="Comapny" required>
                    </div>
                    <div class="mb-2">
                        <input v-model="contact.title" type="text" class="form-control" placeholder="Title" required>
                    </div>
                    <div class="mb-2">
                        <select v-model="contact.groupId" class="form-control" v-if="groups.length > 0" required>
                            <option value="">Select Group</option>
                            <option :value="group.id" v-for="group of groups" :key="group.id">{{ group.name }}</option>
                        </select>
                    </div>
                    <div class="mb-2">
                        <input type="submit" class="btn btn-success" value="Update">
                    </div>
                </form>
            </div>
            <div class="col-md-4">
                <img :src="contact.photo" alt="..." class="contact-img">
            </div>
        </div>
    </div>
</template>
  
<script>
import { ContactService } from '@/service/ContactService';

export default {
    name: 'EditContact',
    data: function () {
        return {
            contactId: this.$route.params.contactId,
            loading: false,
            errorMessage: null,
            contact: {
                name: '',
                email: '',
                mobile: '',
                company: '',
                photo: '',
                title: '',
                groupId: ''
            },
            groups: []
        }
    },
    created: async function () {
        try {
            this.loading = true
            let res = await ContactService.getContacts(this.contactId)
            let groupRes = await ContactService.getAllGroups(res.data)
            this.contact = res.data
            this.groups = groupRes.data
            this.loading = false
        } catch (error) {
            this.errorMessage = error
            this.loading = false
        }
    },
    methods: {
        updateSubmit: async function () {
            try {
                let res = await ContactService.updateContacts(this.contact, this.contactId)
                if (res) {
                    return this.$router.push('/') //redirect to contact manager page
                } else {
                    return this.$router.push('/contacts/edit/${this.contactId}')
                }
            } catch (error) {
                console.log(error);
            }
        }
    }
}
</script>
  
<style></style>
  