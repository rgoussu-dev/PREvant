/*-
 * ========================LICENSE_START=================================
 * PREvant Frontend
 * %%
 * Copyright (C) 2018 - 2019 aixigo AG
 * %%
 * Permission is hereby granted, free of charge, to any person obtaining a copy
 * of this software and associated documentation files (the "Software"), to deal
 * in the Software without restriction, including without limitation the rights
 * to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 * copies of the Software, and to permit persons to whom the Software is
 * furnished to do so, subject to the following conditions:
 *
 * The above copyright notice and this permission notice shall be included in
 * all copies or substantial portions of the Software.
 *
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 * OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
 * THE SOFTWARE.
 * =========================LICENSE_END==================================
 */

<template>
   <div class="container" id="app">

      <spinner v-if="isFetchInProgress" />

      <div v-if="errors.length > 0" class="alert alert-danger" role="alert">
         <p v-for="error in errors">
            <b>{{ error.title }}</b>: {{ error.detail }}
         </p>
      </div>
      <h1 v-else-if="reviewApps.length === 0" class="ra-container__title--preview">
         No apps to review.
      </h1>

      <h1 class="ra-container__title--preview" v-if="myApps.length > 0">My Previews</h1>
      <transition-group tag="div" name="list-complete" class="ra-container__apps--preview ra-apps ">
         <review-app-card
            v-for="reviewApp in myApps"
            :key="reviewApp.name"
            :review-app="reviewApp"
            :showOwners="issuers != null"
            v-on:changeState="changeServiceState"
            class="list-complete-item"/>
      </transition-group>

      <h1 class="ra-container__title--feature" v-if="appsWithoutTicket.length > 0">Previews</h1>
      <transition-group tag="div" name="list-complete" class="ra-container__apps--preview ra-apps ">
         <review-app-card
            v-for="reviewApp in appsWithoutTicket"
            :key="reviewApp.name"
            :review-app="reviewApp"
            :showOwners="issuers != null"
            v-on:changeState="changeServiceState"
            class="list-complete-item"/>
      </transition-group>

      <h1 class="ra-container__title--feature" v-if="appsWithTicket.length > 0">Features</h1>
      <transition-group tag="div" name="list-complete" class="ra-container__apps--feature ra-apps">
         <review-app-card
            v-for="reviewApp in appsWithTicket"
            :key="reviewApp.name"
            :review-app="reviewApp"
            :showOwners="issuers != null"
            v-on:showLogs="showServiceLogs"
            v-on:changeState="changeServiceState"
            class="list-complete-item"/>
      </transition-group>
   </div>
</template>

<style>
   .list-complete-item {
      transition: all 1s;
   }

   .list-complete-enter, .list-complete-leave-to
      /* .list-complete-leave-active below version 2.1.8 */
   {
      opacity: 0;
      transform: translateY(30px);
   }

   .list-complete-leave-active {
      position: absolute;
   }

   .alert > p {
      margin-bottom: 0;
      text-align: center;
   }
   .alert > p + p {
      margin-top: 1rem;
   }
</style>

<script>
   import { mapGetters } from 'vuex';
   import LogsDialog from './LogsDialog.vue';
   import ReviewAppCard from './ReviewAppCard.vue';
   import Spinner from './Spinner.vue';

   export default {
      data() {
         return {};
      },
      components: {
         'review-app-card': ReviewAppCard,
         'logs-dialog': LogsDialog,
         'spinner': Spinner
      },
      computed: {
         ...mapGetters([
            'issuers',
            'reviewApps',
            'appsWithTicket',
            'appsWithoutTicket',
            'myApps',
            'errors',
            'isFetchInProgress'
         ])
      },
      methods: {
         changeServiceState( appName, serviceName ) {
            this.$store.dispatch( 'changeServiceState', { appName, serviceName } );
         }
      }
   };


</script>
