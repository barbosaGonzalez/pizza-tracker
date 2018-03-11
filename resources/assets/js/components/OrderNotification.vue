<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card card-default">
                    <div class="card-header">Order Notification Component</div>

                    <div class="card-body">
                        I'm an example component.
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import BootstrapVue from 'bootstrap-vue'

Vue.use(BootstrapVue);

export default {
    data () {
        return {

        }
    },
    props: ['order_id'],

    mounted() {
        Echo.channel('pizza-tracker.' + this.order_id)
            .listen('OrderStatusChanged', (order) => {
        });
    }
}
</script>
