<template>
   <div>

        <b-alert :show="dismissCountDown"
                dismissible
                variant="success"
                @dismissed="dismissCountDown=0"
                @dismiss-count-down="countDownChanged">
        <p>Order ID:#{{ order_id }} Status Updated!</p>
        <p>I will dissmiss in {{dismissCountDown}} seconds...</p>
        <b-progress variant="success"
                    :max="dismissSecs"
                    :value="dismissCountDown"
                    height="4px">
        </b-progress>
        </b-alert>



    </div>
</template>

<script>

import BootstrapVue from 'bootstrap-vue'

Vue.use(BootstrapVue);

export default {
    data () {
        return {
            dismissSecs: 10,
            dismissCountDown: 0,
            showDismissibleAlert: false,
            order_id: ''
        }
    },
    props: ['user_id'],

    methods: {
        countDownChanged (dismissCountDown) {
            this.dismissCountDown = dismissCountDown
        },
        showAlert () {
            this.dismissCountDown = this.dismissSecs
        }
    },

    mounted() {
        Echo.channel('pizza-tracker')
            .listen('OrderStatusChanged', (pizzaOrder) => {
                if(this.user_id == pizzaOrder.user_id) {
                    this.showAlert();
                    this.order_id = pizzaOrder.id
                }
        });
    }
}
</script>
