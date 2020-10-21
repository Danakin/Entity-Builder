<template>
    <table class="table b-table b-table-caption-top">
        <caption>
            <h3 class="inline mr11">DataBase Schema</h3>
            <b-button v-if="connected" @click="read" variant="outline-primary"> Read </b-button>
            <ConnectButton v-else @connect="load"></ConnectButton>
        </caption>
        <tbody v-if="connected">
            <tr>
                <td>prefix {{ data.prefix }}</td>
            </tr>
            <tr v-for="table in data.tables" :key="table.name">
                <td>
                    <b-form-checkbox v-model="table.included"> {{ table.name }} </b-form-checkbox>
                </td>
            </tr>
        </tbody>
        <tfoot v-if="connected">
            <tr>
                <td>
                    <b-form-checkbox v-model="all" @change="change" class="inline mr11"> All </b-form-checkbox>
                    <b-button @click="convert" variant="outline-primary" class="mr11"> Import selected table </b-button>
                    <span>
                        <b-form-radio v-model="skip" :value="true" class="inline mr11">Skip if exist</b-form-radio>
                        <b-form-radio v-model="skip" :value="false" class="inline">Replace if exist</b-form-radio>
                    </span>
                </td>
            </tr>
        </tfoot>
    </table>
</template>

<script>
import ConnectButton from '../button/ConnectButton.vue'
import { readDB, isConnected, getErrorMessage } from '@/request.js'
import sss from '@/state.js'

export default {
    name: 'TableList',
    components: {
        ConnectButton,
    },
    data() {
        return {
            all: true,
            skip: false,
            connected: isConnected(),
            data: {
                prefix: '',
                tables: [],
            },
        }
    },
    methods: {
        load(data) {
            this.connected = isConnected()
            console.log(data.version)
        },
        read() {
            readDB()
                .then(response => {
                    if (response.data.tables.length) {
                        this.data = response.data
                        this.change(true)
                        return
                    }

                    this.$bvToast.toast('No table found', {
                        title: 'OK',
                        variant: 'success',
                        solid: true,
                    })
                })
                .catch(error => {
                    const message = getErrorMessage(error)
                    this.$bvToast.toast(message, {
                        title: 'i',
                        variant: 'danger',
                        solid: true,
                    })
                })
        },
        convert() {
            if (this.data.tables.length === 0) {
                return
            }

            try {
                sss.convert(this.data, this.skip)
                this.$bvToast.toast('Table imported', {
                    title: 'i',
                    variant: 'success',
                    solid: true,
                })
            } catch (error) {
                this.$bvToast.toast(error.message, {
                    title: 'i',
                    variant: 'danger',
                    solid: true,
                })
            }
        },
        change(value) {
            this.data.tables.forEach(table => {
                table.included = value
            })
        },
    },
}
</script>
