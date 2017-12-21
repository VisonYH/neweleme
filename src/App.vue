<template>
<div class="app">
    <v-head :seller='seller'></v-head>
    <tab></tab>

</div>
</template>

<script type="text/ecmascript-6">
import header from 'components/header/header';
import tab from 'components/tab/tab';
const ERR_OK = 0;
export default {
    components: {
        'v-head': header,
        tab
    },
    created() {
        this.$http.get('/api/seller').then(response => {
            response = response.body;
            if (response.errno === ERR_OK) {
                this.seller = response.data;
            }
        });
    },
    data() {
        return {
            seller: {}
        };
    }
};
</script>

<style lang="stylus" rel="stylesheet/stylus"> 
</style>
