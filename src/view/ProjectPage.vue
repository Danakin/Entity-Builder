<template>
    <div>
        <div>
            <h1 class="inline mr11">Project</h1>
            <b-button-group>
                <b-button @click="download" variant="outline-success"> Download </b-button>
                <b-button v-if="connected" @click="save" variant="outline-success"> Save </b-button>
            </b-button-group>
        </div>
        <ProjectProperty :item="sss.project"></ProjectProperty>
        <TableList></TableList>
    </div>
</template>

<script>
import ProjectProperty from './schema/ProjectProperty.vue'
import TableList from './schema/TableList.vue'
import { isConnected, download, save, getErrorMessage } from '../request.js'
import sss from '../state.js'

export default {
    name: 'ProjectPage',
    components: {
        ProjectProperty,
        TableList,
    },
    data() {
        return {
            sss,
        }
    },
    computed: {
        connected() {
            return isConnected()
        },
    },
    methods: {
        download() {
            try {
                const name = sss.project.fileName
                const text = JSON.stringify(sss.project)
                download(name, text)
            } catch (error) {
                this.$bvToast.toast(error.message, {
                    title: 'i',
                    variant: 'danger',
                    solid: true,
                })
            }
        },
        save() {
            save(sss.project)
                .then(response => {
                    this.$bvToast.toast('Project saved', {
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
    },
}
</script>
