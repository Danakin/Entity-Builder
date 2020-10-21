<template>
    <table class="table b-table b-table-caption-top">
        <caption>
            <h3>Property</h3>
        </caption>
        <thead>
            <tr>
                <th class="text-right" style="width: 111px;">name</th>
                <th>value</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td class="text-right">tableName</td>
                <td>
                    <ChangeButton :item="item" name="tableName"></ChangeButton>
                </td>
            </tr>
        </tbody>
    </table>
</template>

<script>
import ChangeButton from '../button/ChangeButton.vue'

export default {
    name: 'EntityProperty',
    components: {
        ChangeButton,
    },
    props: {
        item: {
            type: Object,
            required: true,
        },
    },
}
</script>
