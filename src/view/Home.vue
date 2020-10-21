<template>
    <div>
        <div class="text-center" style="margin-top: 11%;">
            <img alt="logo" src="@/assets/logo.png" style="width: 222px;" />
        </div>

        <div>
            <b-button @click="create" variant="outline-primary"> New </b-button>
        </div>
    </div>
</template>

<script>
import sss from '../state.js'

export default {
    name: 'Home',
    methods: {
        create() {
            sss.create()
        },
    },
}
</script>
