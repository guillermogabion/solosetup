<template>
    <v-navigation-drawer
          app
          v-model="mini"
          persistent
      >
          <v-list-item class="px-2 pt-2">
              <v-img class="at-logo" src="/images/logo.png"></v-img>
          </v-list-item>
          <v-list >
              <v-list-item
                  v-for="item in items"
                  :key="item.title"
                  color="secondary"
                  :class="{
                      'v-list-item--active': checkIfActive(item.route)
                  }"
                  @click.stop="$router.push(item.route)"
              >
                  <v-list-item-icon>
                      <v-icon>{{ item.icon }}</v-icon>
                  </v-list-item-icon>
  
                  <v-list-item-content>
                      <v-list-item-title>{{ item.title }}</v-list-item-title>
                  </v-list-item-content>
              </v-list-item>
          </v-list>
          <template v-slot:append>
              <v-list-item
                  color="secondary"
                  @click.stop="logoutAdmin"
              >
                  <v-list-item-icon>
                      <v-icon>mdi-logout-variant</v-icon>
                  </v-list-item-icon>
  
                  <v-list-item-content>
                      <v-list-item-title>Log-out</v-list-item-title>
                  </v-list-item-content>
              </v-list-item>
          </template>
      </v-navigation-drawer>
  </template>
  <script>
  export default {
      data: () => ({
          drawer: true,
          items: [
              { title: 'Dashboard', icon: 'mdi-view-dashboard', route: '/dashboard' },
         
          ],
      }),
      props : {
          mini : {
              require: true,
              type : Boolean
          }
      },
      computed : {
          activeRoute () {
              return this.$route
          },
          mini_status: {
              get: function() {
                  return this.mini
              },
              set: function(value) {
                  console.log(value,"status")
                  this.$emit('changeStatusDrawer')
              }
          }
      },
      methods : {
          logoutAdmin(){
                  this.$admin.post('api/logout').then((response) => {
                      localStorage.removeItem("token")
                      localStorage.removeItem('user_type')
                      this.$router.push('/login')
                  })
          },
          checkIfActive(route){
              let route_text = route.split("/")
              return route_text[1].includes(this.activeRoute.name)
          }
      },
      
  }
  </script>