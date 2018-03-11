<template>
    <div>
        <div>
            <b-progress :precision="2" class="mb-3" :value="progress" variant="success" striped :animated="animate"></b-progress>
        </div>

        <div class="order-status">
            <strong>Order Status: </strong> {{ statusNew }}
        </div>

        <img src="http://bestanimations.com/Food/FastFood/Pizza/pizza-animated-gif-55.gif" alt="delivery" v-if=" progress >= 100">
    </div>
        
</template>

<script>

import BootstrapVue from 'bootstrap-vue'

Vue.use(BootstrapVue);

export default {
    data () {
        return {
            max: 50,
            animate: true,
            statusNew: this.status,
            progress: JSON.parse(this.initial)
        }
    },
    props: ['status', 'initial', 'order_id'],

    mounted() {
        Echo.channel('pizza-tracker.' + this.order_id)
            .listen('OrderStatusChanged', (order) => {
                this.statusNew = order.status_name
                this.progress = order.status_percent
        });
    }
}
</script>
