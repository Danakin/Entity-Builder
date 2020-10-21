<template>
    <div>
        <div>
            <h1 class="inline mr11">Project</h1>
            <b-button @click="download" variant="outline-success"> Download </b-button>
        </div>
        <ProjectProperty :item="sss.project"></ProjectProperty>
        <div id="axx"></div>
    </div>
</template>

<script>
import ProjectProperty from './schema/ProjectProperty.vue'
import sss from '../state.js'

export default {
    name: 'ProjectPage',
    components: {
        ProjectProperty,
    },
    data() {
        return {
            sss,
        }
    },
    methods: {
        download() {
            try {
                const result = JSON.stringify(sss.project)
                let aaa = document.createElement('a')
                aaa.href = 'data:text/json;charset=utf-8,' + encodeURIComponent(result)
                aaa.download = 'preset.json'
                aaa.innerHTML = 'Download'
                document.getElementById('axx').appendChild(aaa)
            } catch (error) {
                this.$bvToast.toast(error.message, {
                    title: 'i',
                    variant: 'danger',
                    solid: true,
                })
            }
        },
    },
}
</script>
