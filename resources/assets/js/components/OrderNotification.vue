<template>
        <li class="nav-item dropdown">
        <a id="navbarDropdown" class="nav-link dropdown-toggle" href="#" role="button" data-toggle="dropdown" aria-haspopup="true"
            aria-expanded="false">
            <i class="fa fa-bell"></i>
            <span class="" v-if="notifications.length > 0">{{ notifications.length }}</span>
            <span class="caret"></span>
        </a>

        <div class="dropdown-menu" aria-labelledby="navbarDropdown">
            <li v-for="notification in notifications">
                <a class="dropdown-item py-3" :href="notification.url">
                    <i class="fa fa-exclamation-circle fa-fw"></i> {{ notification.description }}
                    <span class="pull-right"> <timeago :since="notification.time" :auto-update="5"></timeago></span>        
                </a>
            </li>
            
            <div class="dropdown-divider"></div>
            <a class="dropdown-item text-center" href="" v-if="notifications.length > 0">
                <strong>Show all notifications</strong> 
            </a>
            <a class="dropdown-item text-center" href="" v-else>
                <strong>Show all notifications</strong> 
            </a>
        </div>
    </li>
</template>

<script>

import VueTimeago from 'vue-timeago'

Vue.use(VueTimeago, {
  name: 'timeago', // component name, `timeago` by default
  locale: 'en-US',
  locales: {
    // you will need json-loader in webpack 1
    'en-US': require('vue-timeago/locales/en-US.json')
  }
})

export default {
    data () {
        return {
            notifications: []
        }
    },
    props: ['user_id'],

    mounted() {
        Echo.channel('pizza-tracker')
            .listen('OrderStatusChanged', (pizzaOrder) => {
                if(this.user_id == pizzaOrder.user_id) {
                    this.notifications.unshift({
                        description: 'Order ID: #' + pizzaOrder.id + ' updated',
                        url: '/orders/' + pizzaOrder.id,
                        time: new Date()
                    })
                }
        });
    }
}
</script>
