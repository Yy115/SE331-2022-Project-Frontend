<template>
  <div class="background" v-if="isAdmin">
    <div class="home">
      <h1 class="aaa">The Doctor List</h1>
      <div class="doctors">
        <DoctorListItem
          v-for="doctor in doctors"
          :key="doctor.id"
          :doctor="doctor"
        />
      </div>
      <router-link
        id="page-prev"
        :to="{
          name: 'doctorhome',
          query: { page: page - 1 }
        }"
        rel="prev"
        v-if="page != 1"
      >
        Prev Page
      </router-link>
      <span style="color: aliceblue">{{ this.page }}</span>
      <router-link
        id="page-next"
        :to="{
          name: 'doctorhome',
          query: { page: page + 1 }
        }"
        rel="next"
        v-if="hasNextPage"
      >
        Next Page
      </router-link>
    </div>
  </div>
  <div v-else-if="isDoctor">
    <h4>Go to your own page</h4>
    <router-link
      :to="{ name: 'DoctorDetail', params: { id: GStore.currentUser.id } }"
      >Jump</router-link
    >
  </div>
</template>

<script>
// @ is an alias to /src
import DoctorListItem from '@/components/DoctorListItem.vue'
import DoctorService from '@/services/DoctorService.js'
import AuthService from '@/services/AuthService.js'
export default {
  name: 'DoctorListView',
  inject: ['GStore'],
  props: {
    page: {
      type: Number,
      required: true
    }
  },
  components: {
    DoctorListItem
  },
  data() {
    return {
      doctors: null,
      totalitems: 0
    }
  },
  // eslint-disable-next-line no-unused-vars
  beforeRouteEnter(routeTo, routeFrom, next) {
    DoctorService.getDoctors(2, parseInt(routeTo.query.page) || 1)
      .then((response) => {
        console.log(response.data)
        next((comp) => {
          comp.doctors = response.data
          comp.totalitems = response.headers['x-total-count']
        })
      })
      .catch(() => {
        next({ name: 'NetworkError' })
      })
  },
  // eslint-disable-next-line no-unused-vars
  beforeRouteUpdate(routeTo, routeFrom, next) {
    DoctorService.getDoctors(2, parseInt(routeTo.query.page) || 1)
      .then((response) => {
        this.doctors = response.data
        this.totalitems = response.headers['x-total-count']
        next()
      })
      .catch(() => {
        next({ name: 'NetworkError' })
      })
  },
  computed: {
    hasNextPage() {
      let totalPages = Math.ceil(this.totalitems / 2)
      return this.page < totalPages
    },
    isDoctor() {
      return AuthService.hasRoles('ROLE_DOCTOR')
    },
    isAdmin() {
      return AuthService.hasRoles('ROLE_ADMIN')
    }
  }
}
</script>
<style scoped>
.home-list {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.doctors {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.doctor-card:hover {
  transform: scale(1.01);
  box-shadow: 0 3px 12px 0 rgba(0, 0, 0, 0.2);
}

.pagination {
  display: flex;
  width: 290px;
}
.pagination a {
  flex: 1;
  text-decoration: none;
}
#page-prev {
  text-align: left;
  margin-right: 100px;
  padding-bottom: 100px;
  color: aliceblue;
}

#page-next {
  text-align: right;
  margin-left: 100px;
  padding-bottom: 100px;
  color: aliceblue;
}
</style>
