<template>
  <div>
      <div class="container">
            
            <h2 class="title mt-5">Company List</h2>

            <nav class="panel">
                <div class="panel-block">
                    <p class="control has-icons-left"><input class="input" placeholder="Search Company" type="text" @keyup="searchCompany" v-model="search"> 
                    <span class="icon is-left"><i aria-hidden="true" class="fa fa-search"></i></span></p>
                </div>
                
                <div class="panel-block" v-for="(company, index) in companyList" :key="index">
                    <div class="column is-6" v-if="company">
                        <div class=""><b>Company Name: </b>{{ company.name }}</div>
                        <div class=""><b>Phone Number: </b>{{ company.phone_number }}</div>
                        <div class=""><b>Address: </b>{{ company.address }}</div>
                    </div>
                    <div class="column is-6 has-text-right">
                        <Favorite :company="company.id" :favorited="company.favorited"></Favorite>
                    </div>
                </div>
                
            </nav>
      </div>
  </div>
</template>

<script>
    import Favorite from '@/components/Favorite.vue'
    import axios from 'axios'
    import {mapGetters} from 'vuex'

    export default {
        
        name: 'Dashboard',
            components: {
            Favorite
        },
        computed: {
        ...mapGetters({
                authenticated: 'auth/authenticated',
                user: 'auth/user'
            }), 
        },
        data() {
            return {
                companyList: [],
                search: ''
            }
        },
        created() {
            this.companies()
        },
        methods: {
            // Featch company List
            async companies() {
            let response = await axios.get('/company-list');
            this.companyList = response.data.data
            
            },
            // search company List
            async searchCompany() {
                let response = await axios.get('/company/search?q='+this.search);
                this.companyList = response.data.data

            }
        }

    }

</script>
