<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-success fw-bold">View Contact</p>
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

    <div class="container" v-if="!loading && isDone()">
        <div class="row align-items-center">
            <div class="col-md-4">
                <img :src="contact.photo" alt="..." class="contact-img-big">
            </div>
            <div class="col-sm-6">
                <ul class="list-group">
                    <li class="list-group-item">Name : <span class="fw-bold">{{ contact.name }}</span></li>
                    <li class="list-group-item">Email : <span class="fw-bold">{{ contact.email }}</span></li>
                    <li class="list-group-item">Mobile : <span class="fw-bold">{{ contact.mobile }}</span></li>
                    <li class="list-group-item">Comapny : <span class="fw-bold">{{ contact.company }}</span></li>
                    <li class="list-group-item">Title : <span class="fw-bold">{{ contact.title }}</span></li>
                    <li class="list-group-item">Group : <span class="fw-bold">{{ group.name }}</span></li>
                </ul>
            </div>
        </div>

        <div class="row mt-3">
            <div class="col">
                <router-link to="/" class="btn btn-success"><i class="fa fa-arrow-alt-circle-left"></i> Back</router-link>
            </div>
        </div>
    </div>
</template>
  
<script>
import SpinnerVue from '@/components/SpinnerVue.vue';
import { ContactService } from '@/service/ContactService';

export default {
    name: "ViewContact",
    data: function () {
        return {
            contactId: this.$route.params.contactId,
            loading: false,
            contact: {},
            errorMessage: null,
            group: {}
        };
    },
    created: async function () {
        try {
            this.loading = true;
            let res = await ContactService.getContacts(this.contactId);
            let groupRes = await ContactService.getGroups(res.data);
            this.contact = res.data;
            this.group = groupRes.data;
            this.loading = false;
        }
        catch (error) {
            this.errorMessage = error;
            this.loading = false;
        }
    },
    components: { SpinnerVue },
    methods : {
        isDone : function() {
            return Object.keys(this.contact).length > 0 && Object.keys(this.group).length > 0
        }
    }
}
</script>
  
<style></style>
  